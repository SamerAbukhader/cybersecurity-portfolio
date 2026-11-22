---
layout: home
title: Home
---

## About me

I'm Samer Abukhader, a Master of Cyber Security student at Universiti Malaya. I hold a Bachelor of Computer Science in Software Engineering, so I came into security from the builder's side: I spent my degree designing and shipping software, and that made me curious about how the same systems fail when someone attacks them on purpose. Writing code taught me that most weaknesses are ordinary mistakes, such as an unchecked input, a default password or a service nobody remembered was running. I want to be the person who finds those mistakes before an attacker does, and who can explain the fix to the developers who have to make it. This portfolio records what I am learning in the WQE7001 Cyber Security course, starting with my own isolated lab, and it will grow as I add projects.

## Technical skills

| Skill | Level | Demonstrated in |
| --- | --- | --- |
| Software development (Python, web applications) | Confident | Bachelor's degree in Software Engineering |
| Git and GitHub (commits, branches, pull requests, Pages) | Working knowledge | [This repository](https://github.com/SamerAbukhader/cybersecurity-portfolio) |
| Networking fundamentals (TCP/IP, ports, DNS) | Working knowledge | [Nmap service scan](projects/nmap-service-scan/) |
| Linux command line | Working knowledge | [Virtual lab setup](projects/virtual-lab-setup/) |
| Virtualisation and isolated lab networks | Beginner | [Virtual lab setup](projects/virtual-lab-setup/) |
| Network scanning with Nmap | Beginner | [Nmap service scan](projects/nmap-service-scan/) |

## Certifications and training

**Earned**

- No security certifications earned yet.

**In progress and planned**

- Master of Cyber Security, Universiti Malaya. *In progress*, expected completion TODO.
- ISC2 Certified in Cybersecurity (CC). *Planned*, target April 2027.
- CompTIA Security+. *Planned*, target August 2027.
- TryHackMe SOC Level 1 learning path. *Planned*, target February 2027.

## Projects

### 1. [Virtual lab setup](projects/virtual-lab-setup/)

An isolated two-machine lab (Kali Linux attacker, Metasploitable 2 target) on a host-only network, with proof that it cannot reach my home network or the internet.

### 2. [Nmap service scan](projects/nmap-service-scan/)

A service and version scan of the lab target that found 23 open ports, including a root shell on port 1524 and two backdoored services. Each port is explained from a defender's point of view, with the three I would fix first.

## Learning journey

When I started WQE7001 I thought of security mostly as a coding problem. After a software engineering degree, my instinct was that secure systems come from careful developers. This course has widened that view: security is also about law, networks, monitoring and people, and code is only one part of it.

The first topic that changed how I think was ethics and Malaysian law. Before the course I assumed that scanning a machine was harmless as long as I did not break anything. Studying the Computer Crimes Act 1997 corrected that. Unauthorised access is an offence, and so is an attempt, which means a port scan against a system I do not own could already be a problem. What I took from this is that authorisation and scope come before any tool. It is the reason every project on this site runs on virtual machines that I own, on a network that cannot reach anything else.

The second was building a virtual lab and scanning it with Nmap. Setting up an attacker machine and a deliberately vulnerable target taught me more about networking than any diagram had. The main lesson was that an open port is only the start of the story. Version detection is what turns "port 21 is open" into "this is an FTP server version with a known backdoor". I also learned to read a scan the way a defender would, by asking which three services I would close or patch first and why.

One thing that went wrong came while building this portfolio. My first attempt to commit failed with the error "git is not recognized as the name of a cmdlet". I assumed Git was already on my machine because the GitHub CLI was installed and logged in. To diagnose it I ran the version check for each tool separately: `gh` answered and `git` did not, which showed they are separate programs and that only one was installed. I fixed it by installing Git for Windows and opening a new terminal so the updated PATH was loaded. The lesson was to check my assumptions about my own tools before blaming the command.

The area I still find challenging is log analysis. I can read the output of a single tool, but I have not yet worked hands-on with a SIEM, and I find it hard to connect events from several sources into one timeline. That is the core skill of the SOC analyst role I am aiming for, so it is the gap I most need to close.

My concrete goal is to complete the TryHackMe SOC Level 1 learning path by 28 February 2027. I will measure it in two ways: the path completion certificate, which I will link in the certifications section above, and at least two new project write-ups on this site, one of which must use a Wazuh SIEM in my lab to investigate alerts that I generate myself.

## Career objective

My goal is to become a cyber security specialist and to keep advancing in the field. My first target role is SOC analyst (Tier 1) in Malaysia. A security operations centre is the most realistic entry point for a graduate, and it builds the detection and investigation experience that later specialisms depend on. My software engineering background should help me read logs, script repetitive checks and understand how applications are attacked.

My next steps are:

1. Complete the TryHackMe SOC Level 1 path and add a Wazuh SIEM to my home lab by February 2027.
2. Pass ISC2 Certified in Cybersecurity by April 2027, then CompTIA Security+ by August 2027.
3. Apply for a SOC internship or junior analyst role in late 2027, and enter at least one CTF each semester.

In the longer term I want to move from monitoring into incident response.

## Contact information

- **Email:** [sakhader_2003@hotmail.com](mailto:sakhader_2003@hotmail.com)
- **GitHub:** [SamerAbukhader](https://github.com/SamerAbukhader)
- **LinkedIn:** [linkedin.com/in/samerabukhader](https://www.linkedin.com/in/samerabukhader/)
