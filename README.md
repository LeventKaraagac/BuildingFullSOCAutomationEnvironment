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
<h3> Step 1: Virtual Machine Setup </h3>
1. D
<br>
<img src="" alt="Disk Sanitization Steps"/>
<br>
  
<br>
<img src="" alt="Disk Sanitization Steps"/>
<br>
