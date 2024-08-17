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
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

### Part 1: Create a diagram (to map out how to build lab logically): [Diagram](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/automation-diagram.drawio.png)

### Part 2: Installing Windows 10 Server w/Sysmon, Wazuh Server, and TheHive Server

- Software and Tools needed: A Virtual Machine (VM), Windows 10 identical storage image (iso) file, and Cloud servers (digital ocean).
- VM operating system settings: 50GB of storage and 4GB of RAM.

1. Download [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) zip file from the Microsoft site and the sysmon-modular configuration file [sysmonconfig.xml](https://github.com/olafhartong/sysmon-modular) from github.<br>
Then, unzip the Sysmon folder and move/copy sysmonconfig.xml into it. Next, open Powershell as administrator and install Sysmon64.exe using the sysmonconfig.xml file:
<p align="center"> <img src="https://i.imgur.com/5oL3Ytn.png" align="center"><br> <em>Ref 2.1: sysmon installion using sysmonconfig.xml file</em> </p>
<br>

2. Create both servers for Wazuh and TheHive using the same specifications using Digital Ocean cloud provider. (Save root password in a password manager)<br>
3. Create firewall rules. Navigate to networking --> firewall and set All TCP & UDP Inbound traffic to public IP of host system.
4. Add servers to firewall.
<p align="center"> <img src="https://i.imgur.com/WukDKNE.jpg"><br> <em>Ref 2.2: servers specifications</em> </p>
<p align="center"> <img src="https://i.imgur.com/EHQSOAX.jpg"><br> <em>Ref 2.3: firewall rule creation</em> </p>
<p align="center"> <img src="https://i.imgur.com/o3o2X1u.gif"><br> <em>Ref 2.4: Wazuh & TheHive added to firewall</em> </p>
<br>

5. Install Wazuh - SSH connect to Wazuh server using PuTTy, update and upgrade system using "apt-get update && apt-get upgrade".<br> Follow [Wazuh-Install-Instructions](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/Wazuh-Install-Instructions) to finish setup.<br>
Save username and password generated because this will be used as login to Wazuh dashboard:
<p align="center"> <img src="https://i.imgur.com/QWaGJYG.png"><br> <em>Ref 2.5: username and password generated</em> </p>
<br>

6. Install TheHive - SSH connect to Wazuh server using PuTTy, update and upgrade system using "apt-get update && apt-get upgrade".<br> Follow [TheHive-Install-Instruction](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/TheHive-Install-Instructions) to finish setup.
<br>

### Part 3: Configuring TheHive & Wazuh Server

1. Starting with TheHive, edit the cassandra configuration file ---> nano /etc/cassandra/cassandra.yaml ---> Change:
<p align="center"> <img src="https://i.imgur.com/HZEEnoJ.png" align="center"><br> <em>Ref 3.1: Edit cassandra config file</em> </p>
<br>

2. Stop cassandra.service, remove all old files, start cassandra.service, and check the status of cassandra.service to make sure it's up & running.
<p align="center"> <img src="https://i.imgur.com/4uk1GPS.png" align="center"><br> <em>Ref 3.2: Stop service, remove old files, start and check status of service</em> </p>
<br>

3. Edit the elasticsearch configuration file ---> nano /etc/elasticsearch/elasticsearch.yml ---> Change:
<p align="center"> <img src="https://i.imgur.com/3aW59gD.png" align="center"><br> <em>Ref 3.3: Edit elasticsearch config file</em> </p>
<br>

4. Stop elasticsearch.service, remove all old files, start & enable elasticsearch, and check the status of elasticsearch.service to make sure it's up & running.
<p align="center"> <img src="https://i.imgur.com/b0czGgI.png" align="center"><br> <em>Ref 3.4: Stop service, remove old files, start, enable and check status of service</em> </p>
<br>

5. Now TheHive service, the ownership of /opt/thp needs to be switched from 'root' to 'thehive' (stated in the application.conf file you will next):
<p align="center"> <img src="https://i.imgur.com/8OSX5La.png" align="center"><br> <em>Ref 3.5: Giving thehive ownership of /opt/thp directory</em> </p>
<br>

6. Edit the thehive configuration file ---> nano /etc/thehive/application.conf ---> Change:
<p align="center"> <img src="" align="center"><br> <em>Ref 3.6: Edit thehive config file</em> </p>
<br>

7. Stop thehive.service, remove all old files, start & enable thehive, and check the status of thehive.service to make sure it's up & running.
<p align="center"> <img src="https://i.imgur.com/mLKLGgA.png" align="center"><br> <em>Ref 3.7: Stop service, remove old files, start, enable and check status of service</em> </p>
<br> 

- Double check all services are running and navigate to the public IP using port 9000 on web browser.
- It's a possibility during login you will receive an Authentication Fail ERROR due to the default credentials. Refer to [TheHive-Install-Instruction](https://github.com/kennedyshearer/SOC-Automation-Project/blob/main/TheHive-Install-Instructions) for help.

8. Navigate to TheHive on the browser with 'http://thehive-public-ip:9000' and login with default credentials:
<p align="center"> <img src="https://i.imgur.com/Wxm3Gnf.png" align="center"><br> <em>Ref 3.8: Navigate to TheHive with browser & login</em> </p>
<br> 

9. Login to Wazuh dashboard ---> click Add Agent ---> Fill in choices:
<p align="center"> <img src="https://i.imgur.com/UIAt5ui.png" align="center"><br> <em>Ref 3.9: Creating agent in wazuh dashboard</em> </p>
<br> 

### Part 4: Generate Telemetry & Ingest into Wazuh

1. To start generating Windows 10 telemetry, modify file "C:\\Programs File(x86)\ossec-agent/ossec.conf"(copy a backup). Under Log Analysis add a new <localfile> for Sysmon which PATH can be found through Event Viewer and restart Wazuh service:
<p align="center"> <img src="https://i.imgur.com/rzMUw2a.gif" align="center"><br> <em>Ref 4.1: Find Sysmon PATH</em> </p>
<p align="center"> <img src="https://i.imgur.com/HwO8h4L.png" align="center"><br> <em>Ref 4.1.2: Add <localefile> to ossec.conf</em> </p>
<br>

2. Setting up Wazuh dashboard to ingest mimikatz, start by adding Downloads folder to Windows Defender exclusions, downloading <a href="https://github.com/gentilkiwi/mimikatz/releases/tag/2.2.0-20220919">mimikatz.exe</a> and extracting it in downloads folder:
<p align="center"> <img src="https://i.imgur.com/GdTnprT.png" align="center"><br> <em>Ref 4.2: Adding Downloads folder to exclusions</em> </p>

3. Now to bypass Wazuh default configurations/rules, modify /var/ossec/etc/ossec.conf(create backup) in Wazuh-Manager-CLI to accept all logs:
<p align="center"> <img src="https://i.imgur.com/GUQ7xHf.png" align="center"><br> <em>Ref 4.3: Modify Wazuh-Manager-CLI ossec.conf</em> </p>

4. Restart Wazuh Manager, this will force Wazuh to archive all logs in /var/ossec/logs/archives. To start the ingestion process, filebeats needs to be modified in /etc/filebeat/filebeat.yml, and restarted:
<p align="center"> <img src="https://i.imgur.com/kWfJlLi.png" align="center"><br> <em>Ref 4.4: Modify filebeat.yml</em> </p>
<br>

5. In Wazuh dashboard, create New Index by clicking Stack Management --> Index Pattern --> Create Index Pattern (name: wazuh-archives-* / time_field: timestamp)
<p align="center"> <img src="https://i.imgur.com/J8Tdqve.gif" align="center"><br> <em>Ref 4.5: Create Index Pattern</em> </p>
<br>

6. Go to Discovery tab, select wazuh-archives-*


*Ref 1: Network Diagram*
