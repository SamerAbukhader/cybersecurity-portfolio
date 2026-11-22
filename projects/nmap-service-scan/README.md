---
layout: page
title: Nmap service scan
permalink: /projects/nmap-service-scan/
---

[Back to portfolio]({{ site.baseurl }}/)

## Objective

Find out which network services the target in my [virtual lab]({{ site.baseurl }}/projects/virtual-lab-setup/) exposes, identify their versions, and decide, as a defender would, which ones are the most urgent to close or patch.

**Authorisation:** the target is a Metasploitable 2 VM that I own, on an isolated host-only network. No other system was scanned. Scanning systems without permission is an offence under Malaysia's Computer Crimes Act 1997.

## Environment and tools

| Item | Details |
| --- | --- |
| Scanner | Kali Linux 2026.2, 192.168.56.102 |
| Tool | Nmap |
| Target | Metasploitable 2, 192.168.56.101 |
| Network | Isolated VirtualBox host-only network, 192.168.56.0/24 |

## Method

1. Confirmed the target's address (`ifconfig` on Metasploitable) and that Kali could reach it (`ping`), as shown in the [lab project]({{ site.baseurl }}/projects/virtual-lab-setup/).
2. Ran a service and version scan from Kali:

   ```bash
   nmap -n -sV -oN scan.txt 192.168.56.101
   ```

   - `-sV` probes each open port to identify the software and version behind it. Without it, Nmap only guesses the service from the port number.
   - `-n` skips DNS lookups, which are pointless in an isolated lab and slow the scan down.
   - `-oN scan.txt` saves the result as a text file.
3. Read the output port by port, and checked well-known versions against the Rapid7 *Metasploitable 2 Exploitability Guide*.

The full output is saved as [scan.txt](evidence/scan.txt).

## Results

The scan took 11.5 seconds. Of the 1,000 most common TCP ports, **23 were open** and 977 were *closed*. *Closed* means the host answered with a TCP reset: the port is reachable, but nothing is listening. There were no *filtered* ports, which means no firewall was dropping probes. For a real server, a host with no filtering and 23 open services would be a serious finding in itself.

| Port | Service | Version | What it means for a defender |
| --- | --- | --- | --- |
| 21 | FTP | vsftpd 2.3.4 | This exact release was distributed with a backdoor that opens a root shell. Version detection is what reveals it: "FTP is open" sounds harmless, but "vsftpd 2.3.4" is critical. FTP also sends passwords in cleartext |
| 22 | SSH | OpenSSH 4.7p1 | Encrypted remote login, the right protocol, but a very old version that is long out of support |
| 23 | Telnet | Linux telnetd | Remote login with no encryption: usernames, passwords and commands cross the network in plain text. Should be replaced by SSH |
| 25 | SMTP | Postfix | Mail server; a misconfigured one can be abused to relay spam or to discover valid usernames |
| 53 | DNS | ISC BIND 9.4.2 | Old BIND release with known vulnerabilities; a DNS server should not run on a host that does not need it |
| 80 | HTTP | Apache 2.2.8 | Web server hosting deliberately vulnerable web apps; outdated and unencrypted (no HTTPS) |
| 111, 2049 | RPC, NFS | rpcbind 2, NFS 2-4 | File sharing over the network; a badly configured NFS export can let anyone mount the disk |
| 139, 445 | SMB | Samba 3.x-4.x | Windows-style file sharing. Old Samba versions have remote-code-execution flaws, and SMB should never face an untrusted network |
| 512, 513, 514 | rexec, rlogin, rsh | netkit | Legacy "r-services" from the 1980s that trust hosts by address and send everything in cleartext. They have no place on a modern system |
| 1099 | Java RMI | GNU Classpath | Remote Java object registry, which can allow remote code loading if exposed |
| **1524** | **bindshell** | **Metasploitable root shell** | **A root shell listening openly on the network: anyone who connects is root with no password. This is what a system looks like after it has already been compromised** |
| 2121 | FTP | ProFTPD 1.3.1 | A second FTP server, also old; two FTP servers on one host suggests nobody is managing what is installed |
| 3306 | MySQL | 5.0.51a | Database reachable over the network; databases should only listen locally or to the application server |
| 5432 | PostgreSQL | 8.3.x | Same issue as MySQL |
| 5900 | VNC | protocol 3.3 | Remote desktop using an old protocol with weak authentication |
| 6000 | X11 | (access denied) | Graphical display server; access is denied, but it should not be exposed at all |
| **6667** | **IRC** | **UnrealIRCd** | **Some UnrealIRCd downloads were trojaned with a backdoor that runs commands sent by a remote attacker** |
| 8009, 8180 | AJP, HTTP | Apache Tomcat | Java application server and its connector; Tomcat admin pages are often left with default credentials |

Nmap also reported the MAC address prefix `08:00:27`, which belongs to Oracle VirtualBox. That one line tells an attacker the target is a virtual machine.

### Priorities: what I would fix first

1. **Port 1524, the root bindshell.** This is not a vulnerability but an open door. Kill the process, remove whatever starts it, and treat the whole host as compromised: rebuild it from a clean image.
2. **Port 21 (vsftpd 2.3.4) and port 6667 (UnrealIRCd).** Both are known backdoored builds that give remote command execution. Remove them, or replace them with clean, current versions only if the service is actually needed.
3. **Ports 23 and 512 to 514, the cleartext login services.** Disable telnet and the r-services completely and use SSH with key-based login.

## How I would fix the issues found

- **Reduce the attack surface.** Most of these 23 services have no business running on one machine. I would list what the server is actually for and disable everything else.
- **Patch.** Every version found is many years old. A supported operating system and current packages would remove most known vulnerabilities in one step.
- **Add a host firewall.** Allow only the ports that are needed, and only from the networks that need them. Databases (3306, 5432) and file sharing (139, 445, 2049) should never be reachable from outside.
- **Replace cleartext protocols.** SSH instead of telnet and r-services, SFTP instead of FTP, HTTPS instead of HTTP.

**What I would do differently next time:** scan all 65,535 TCP ports with `-p-` instead of the default top 1,000, add a UDP scan (`-sU`) for services like SNMP, and follow up with a vulnerability scanner such as OpenVAS to match each version to its CVEs. I would also run the scan while capturing traffic in Wireshark, to see what the scan looks like from the defender's side.
