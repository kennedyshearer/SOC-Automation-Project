# SOC-Automation-Project

## Objective

The SOC Automation project is aimed to create a Home-Lab with responsive capabilites and a case management system to mimic a real soft environment.

### Skills Learned

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used

- Graph drawing software (draw.io) to build a logical diagram.
- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

<b>Part 1: Create a diagram (to map out how to build lab logically): [Diagram](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/automation-diagram.drawio.png) <br>

<b>Part 2: Installing Windows 10 Server w/Sysmon, Wazuh Server, and TheHive Server.</b>

- Software and Tools needed: A Virtual Machine (VM), Windows 10 identical storage image (iso) file, and Cloud servers (digital ocean).
- VM operating system settings: 50GB of storage and 4GB of RAM.

1. Download [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) zip file from the Microsoft site and the sysmon-modular configuration file [sysmonconfig.xml](https://github.com/olafhartong/sysmon-modular) from github.<br>
Then, unzip the Sysmon folder and move/copy sysmonconfig.xml into it. Next, open Powershell as administrator and install Sysmon64.exe using the sysmonconfig.xml file:
<p align="center"> <img src="https://i.imgur.com/5oL3Ytn.png" align="center"><br> <em>Ref 1: sysmon installion using sysmonconfig.xml file</em> </p>

2. Create both servers for Wazuh and TheHive using the same specifications using Digital Ocean cloud provider. (Save root password in a password manager)<br>
3. Create firewall rules. Navigate to networking --> firewall and set All TCP & UDP Inbound traffic to public IP of host system.
4. Add servers to firewall.
<p align="center"> <img src="https://i.imgur.com/WukDKNE.jpg"><br> <em>Ref 2: servers specifications</em> </p>
<p align="center"> <img src="https://i.imgur.com/EHQSOAX.jpg"><br> <em>Ref 3: firewall rule creation</em> </p>
<p align="center"> <img src="https://i.imgur.com/o3o2X1u.gif"><br> <em>Ref 4: Wazuh & TheHive added to firewall</em> </p>

5. Install Wazuh - SSH connect to Wazuh server using PuTTy, update and upgrade system using "apt-get update && apt-get upgrade".<br> Follow [Wazuh-Install-Instructions](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/Wazuh-Install-Instructions) to finish setup.<br>
Save username and password generated because this will be used as login to Wazuh dashboard:
<p align="center"> <img src="https://i.imgur.com/QWaGJYG.png"><br> <em>Ref 5: username and password generated</em> </p>

6. Install TheHive - SSH connect to Wazuh server using PuTTy, update and upgrade system using "apt-get update && apt-get upgrade".<br> Follow [TheHive-Install-Instruction](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/TheHive-Install-Instructions) to finish setup.


<b>Part 3: Configuring TheHive & Wazuh Server.</b>



*Ref 1: Network Diagram*
