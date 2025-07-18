<p align="center">
<img src="https://i.imgur.com/ZFU2Ea2.png" height="800px" width="60%" alt="Basic Home SOC Lab Steps"/>
</p>

<br>
<h1>Building a Full SOC Automation Environment with Wazuh, TheHive and Shuffle SOAR</h1>


<h2>Project Description</h2>
This project outlines the establishment of a robust Security Operations Center (SOC) automation environment, primarily hosted on DigitalOcean's cloud platform. The architecture integrates Wazuh for endpoint security monitoring, TheHive for collaborative incident response, and Shuffle as a Security Orchestration, Automation, and Response (SOAR) platform. The core objective is to demonstrate end-to-end automation, from threat detection and alert generation to automated threat intelligence lookups (via VirusTotal) and incident creation in TheHive, including email notifications. A simulated attack is used to showcase the automated workflow and capabilities of the integrated environment.



<h2>Environments and Technologies Used</h2>

- <b>Virtualization:</b> VirtualBox, and DigitalOcean(Cloud)
- <b>Endpoint Security Monitoring:</b> Wazuh (Manager and Agent)
- <b>Incident Response Platform:</b> The Hive
- <b>Security Orchestration, Automation, and Response (SOAR):</b> Shuffle
- <b>Threat Intelligence:</b> VirusTotal API


<h2>Operating Systems Used</h2>

- <b>Ubuntu Server 22.04 LTS:</b> Used for the DigitalOcean Droplets hosting Wazuh Manager, TheHive, and Shuffle.
- <b>Windows 10 VM:</b> Used as an endpoint machine where the Wazuh agent is installed and attack simulations are conducted.



<h2>Overview of Steps</h2>
1. <b>Cloud Infrastructure Setup:</b> Provisioning and initial configuration of two Ubuntu Droplets on DigitalOcean for Wazuh and TheHive.
<br>
2. <b>Wazuh Deployment:</b> Installation and configuration of the Wazuh Manager, followed by the deployment and registration of a Wazuh agent on a Windows endpoint.
<br>
3. <b>TheHive Deployment:</b> Installation and setup of TheHive incident response platform, including user management.
<br>
4. <b>Shuffle SOAR Setup:</b> Accessing and configuring the Shuffle SOAR platform as a web service.
<br>
5. <b>Integration Components:</b> Connecting Wazuh alerts to Shuffle for automated ingestion. Integrating VirusTotal into Shuffle workflows for automated threat intelligence lookups (e.g., file hash reputation). Connecting Shuffle to TheHive for automated alert creation and incident management.
<br>
6. <b>Attack Simulation and Automation Demonstration:</b> Executing a simulated attack (e.g., Mimikatz) on the monitored Windows endpoint to trigger alerts and demonstrate the full automation workflow through Wazuh, Shuffle, and TheHive.



<h2>Project Walkthrough</h2>
<h3> Step 1: Cloud Infrastructure Setup (DigitalOcean) </h3>
1. Create DigitalOcean Droplets: Create the first Ubuntu Server 22.04 LTS Droplets on DigitalOcean. This Droplet will host the Wazuh Manager, Assign appropriate resources (e.g., CPU, RAM, storage) and select a region.
<br>
<img src="https://i.imgur.com/LFucZB2.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/t7NLiua.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. Create a firewall within DigitalOcean and create a rule that allows only SSH connection inbound from my public IP address. Then add the VM to the firewall from networking settings.
<br>
<img src="https://i.imgur.com/wbHsxMN.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/7OUMJvH.png" alt="Building a SOC Automation Environment Steps"/>
<br>
3. Access Droplet via SSH: Connect to the newly created Ubuntu Droplets using SSH. This is essential for installing and configuring the necessary software Wazuh
<br>
<img src="https://i.imgur.com/KDMtMe0.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3> Step 2: Wazuh Deployment and Configuration</h3>
1. Install Wazuh Manager: Configuration of Wazuh is initiated using the curl command provided on its official website.
<br>
<img src="https://i.imgur.com/DBmt8kK.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. Access Wazuh Dashboard: To log in to Wazuh, a browser is opened, and the Wazuh server is accessed by typing https://<Public IP>.
<br>
<img src="https://i.imgur.com/6AuBwj9.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3> Step 3: TheHive Droplet and Configuration</h3>
1. TheHive Droplet Creation (Ubuntu 22.04): Another Droplet, an Ubuntu 22.04 instance, is created for the second VM, which will host TheHive. This machine is also added to the firewall for protection.
<br>
<img src="https://i.imgur.com/oY0X8hO.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. TheHive SSH Access and Dependency Installation: SSH access is established to this new machine, and TheHive is downloaded. Dependencies such as ElasticSearch, Cassandra, and Java are installed by executing the necessary commands.
<br>
<img src="https://i.imgur.com/ItmRErC.png" alt="Building a SOC Automation Environment Steps"/>
<br>
3. Cassandra File Editing: Cassandra's files are edited with the command nano /etc/cassandra/cassandra/yaml. The listen_address, rpc_address, and seed_address parameters need to be changed.
<br>
<img src="https://i.imgur.com/LdhXyDv.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/BISTCwt.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/tzHOAqe.png" alt="Building a SOC Automation Environment Steps"/>
<br>
4. Cassandra Service Management: The Cassandra service is then stopped, and older files from Cassandra are removed. After removing older Cassandra files, the Cassandra service is restarted.
<br>
<img src="https://i.imgur.com/x7wyg1R.png" alt="Building a SOC Automation Environment Steps"/>
<br>
5. Elasticsearch Configuration: Elasticsearch is configured using the command nano /etc/elasticsearch/elasticsearch.yml to uncomment and change cluster.name, node.name, network.host, http.port, and cluster.initial_master_node.
<br>
<img src="https://i.imgur.com/PJNmuMg.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/y9KpYWo.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/SQLM4tA.png" alt="Building a SOC Automation Environment Steps"/>
<br>
6. Elasticsearch Service Restart: After saving the configuration for the elasticsearch.yml file, the Elasticsearch service is restarted.
<br>
<img src="https://i.imgur.com/QIBNQyX.png" alt="Building a SOC Automation Environment Steps"/>
<br>
7. TheHive Directory Ownership Change: The owner of TheHive's directory is changed using the chown command to ensure TheHive has appropriate access rights.
<br>
<img src="https://i.imgur.com/hXZ29IC.png" alt="Building a SOC Automation Environment Steps"/>
<br>
8. TheHive Configuration File (application.conf): The configuration file of TheHive is configured with the command nano /etc/thehive/application.conf. Database configuration, hostname (to the IP address), index_name, and application base URL are changed.
<br>
<img src="https://i.imgur.com/HDeX1gP.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/jamMTd3.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/wkMh7R4.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/kPZA0te.png" alt="Building a SOC Automation Environment Steps"/>
<br>
9. Start TheHive and access via browser: TheHive is started using the provided commands. TheHive is accessed from a browser by typing https://<IP-Address>:9000.
<br>
<img src="https://i.imgur.com/uL9JyPP.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/8Jyr337.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3> Step 4: Wazuh Agent Configuration</h3>
1. Wazuh Agent Configuration (from Dashboard): Wazuh is configured from the dashboard, with the need to add an agent. During this process, a command provided by Wazuh is pasted into PowerShell to add the agent.
<br>
<img src="https://i.imgur.com/YA3aeoA.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/PF7BGkd.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. Start Wazuh Service: The Wazuh service is started with the provided command. The result of the agent setup can now be seen.
<br>
<img src="https://i.imgur.com/04o2uGi.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/S2waofC.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3> Step 5: Sysmon Log Ingestion and Verification</h3>
1. Sysmon Telemetry Ingestion: To ensure logs are ingested into Wazuh, the ossec.conf config file in the Windows 10 VM (which has the Wazuh agent installed) is modified to ingest Sysmon telemetry. This file is found in the program files of Wazuh.
<br>
<img src="https://i.imgur.com/kaKWdS9.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. Check Sysmon Events on Wazuh Dashboard: Returning to the Wazuh dashboard, Sysmon events are checked.
<br>
<img src="https://i.imgur.com/pI7Dvjd.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3> Step 6: Mimikatz Simulation and Detection</h3>
1. Mimikatz Exclusion Creation: An exclusion is created to allow Mimikatz to be downloaded in the downloads folder.
<br>
<img src="https://i.imgur.com/X77x253.png" alt="Building a SOC Automation Environment Steps"/>
<br>
2. Mimikatz Download: Mimikatz is downloaded from the GitHub page gentilkiwi/mimikatz.
<br>
<img src="https://i.imgur.com/lNuxpED.png" alt="Building a SOC Automation Environment Steps"/>
<br>
3. Mimikatz Execution: Mimikatz is executed by going to the directory where mimikatz resides executing the file with .\mimikatz.exe in powershell.
<br>
<img src="https://i.imgur.com/cmpLnKY.png" alt="Building a SOC Automation Environment Steps"/>
<br>
4. Check Mimikatz Events on Wazuh: Returning to Wazuh, events triggered because of Mimikatz are checked.
<br>
<img src="https://i.imgur.com/LK4lBXB.png" alt="Building a SOC Automation Environment Steps"/>
<br>
5. Mimikatz Alert Rule Creation: An alert rule is created that will trigger whenever Mimikatz is detected.
<br>
<img src="https://i.imgur.com/TOuUyMl.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<h3>Step 7: SOAR Integration and Automated Response</h3>
1. SOAR Platform Connection (Shuffle): With Wazuh and the systems ready, the process now moves to connecting the SOAR platform (Shuffle), sending an alert with TheHive, and sending an email.
2. Shuffle Account and Initial Configuration: A Shuffle account is opened, and the system is configured step by step. The first step involves getting the Wazuh alerts to the SOAR.
<br>
<img src="https://i.imgur.com/wmfw52v.png" alt="Building a SOC Automation Environment Steps"/>
<br>
3. Modify Wazuh ossec.conf for Integration Tags: The ossec.conf config file of Wazuh is modified to change and add integration tags.
<br>
<img src="https://i.imgur.com/MrtxLCv.png" alt="Building a SOC Automation Environment Steps"/>
<br>
4. Verify Alert Reception in SOAR: When Mimikatz is executed again, the alert is successfully received by the SOAR system.
<br>
<img src="https://i.imgur.com/uvO9xZS.png" alt="Building a SOC Automation Environment Steps"/>
<br>
5. Hash Value Extraction with Regex: To obtain the hash value of the file and send it to VirusTotal for reputation checking, a regex is used to select the specific output. Only the sha256 hash is extracted.
<br>
<img src="https://i.imgur.com/V3M9bC2.png" alt="Building a SOC Automation Environment Steps"/>
<br>
6. VirusTotal Integration in Automation Flow: VirusTotal is added to the automation flow and configured to get a hash report.
<br>
<img src="https://i.imgur.com/CwSPKDM.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/zHSCICR.png" alt="Building a SOC Automation Environment Steps"/>
<br>
7. Verify VirusTotal Step: The system is re-run to see if the VirusTotal step is working.
<br>
<img src="https://i.imgur.com/VQpd3Kg.png" alt="Building a SOC Automation Environment Steps"/>
<br>
8. TheHive Integration in Workflow: TheHive is added to the workflow. TheHive is configured to create an alert.
<br>
<img src="https://i.imgur.com/GBDVpGZ.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/9T8ulDz.png" alt="Building a SOC Automation Environment Steps"/>
<br>
9. Workflow Execution Result (TheHive Alert): The result of the workflow when executed and the alert result at TheHive are observed.
<br>
<img src="https://i.imgur.com/IscUKQd.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/ePjmYTz.png" alt="Building a SOC Automation Environment Steps"/>
<br>
10. Email Notification Setup: As the last step of the workflow, an email notification is added. So when the event is triggered by executing Mimikatz again, an incident is received on TheHive, and an email is received at the same time.
<br>
<img src="https://i.imgur.com/xM3JARo.png" alt="Building a SOC Automation Environment Steps"/>
<br>
<img src="https://i.imgur.com/QkdNGJQ.png" alt="Building a SOC Automation Environment Steps"/>
<br>

<h2>Key Learning/Reflections</h2> 

This project offered invaluable hands-on experience in building a scalable and automated SOC environment. Key insights and learnings include:
<br>
- <b>Cloud Infrastructure Management:</b> Gained practical experience in deploying and managing virtual machines (Droplets) on a cloud platform like DigitalOcean, including network configuration and SSH access.
- <b>Multi-Platform Integration:</b> Successfully integrated disparate security tools (Wazuh, TheHive, Shuffle, VirusTotal) to create a cohesive automation pipeline. This highlighted the complexities and benefits of API-driven security operations.
- <b>SOAR Workflow Development:</b> Developed practical skills in designing and implementing automation workflows within Shuffle, including parsing alerts, performing external lookups (VirusTotal), and triggering actions in other platforms (TheHive, email).
- <b>Endpoint Detection & Response (EDR) with Wazuh:</b> Deepened understanding of Wazuh's capabilities for endpoint monitoring, log collection, and rule-based alerting, crucial for initial threat detection.
- <b>Incident Response Automation:</b> Learned how automated incident creation in TheHive can significantly reduce manual effort and accelerate the triage process for security analysts.
- <b> Troubleshooting Integration Challenges:</b> Faced and overcame challenges related to API connectivity, data parsing (e.g., using regex for hash extraction), and permission configurations between various platforms, emphasizing the importance of detailed logging and debugging in complex environments.
<br>

<h2>Conclusion</h2>
This project successfully established a comprehensive SOC automation environment, demonstrating the power of integrating open-source security tools for enhanced threat detection, incident management, and automated response. The end-to-end workflow, from endpoint monitoring with Wazuh to alert orchestration via Shuffle and incident creation in TheHive, showcases a practical approach to modern security operations. The skills acquired in cloud deployment, system integration, and automation logic are directly applicable to optimizing security workflows and improving response times in real-world SOC environments. This project serves as a robust foundation for future exploration into more advanced automation playbooks and security tool integrations.

