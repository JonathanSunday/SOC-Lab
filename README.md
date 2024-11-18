# Building a Security Operations Center (SOC) in Azure with MISP Integration

## Objective

The goal of this project was to set up a Security Information and Event Management (SIEM) system using Microsoft Sentinel to monitor and analyze security events in a home lab environment. Additionally, we integrated the Malware Information Sharing Platform (MISP). This was achieved by setting up MISP in a Docker container on an Ubuntu virtual machine (VM) in Azure, importing threat feeds, and integrating them with Microsoft Sentinel. The environment simulated real-world attack scenarios, including Remote Desktop Protocol (RDP) traffic, and utilized MISP for data collection.


### Skills Learned


- Hands-on implementation of Microsoft Sentinel as a SIEM.
- Implementation and configuration of Microsoft Sentinel as a SIEM in Azure.
- Setting up and managing MISP in a Docker container.
- Integration of threat intelligence from MISP into Sentinel for enhanced detection.
- Monitoring and analyzing RDP logs for signs of security incidents.
- Developed custom analytics rules for threat detection, including brute-force attacks and unauthorized access.
- Practical understanding of Azure resource management, including VMs, resource groups, and security configurations.


### Tools Used

- Microsoft Azure: Virtual machines and resource groups for environment setup.
- Microsoft Sentinel: For log ingestion, analytics, and incident management.
- MISP (Malware Information Sharing Platform): For threat intelligence data ingestion.
- Docker: For running MISP in an isolated container.
- Python: Used for scripting and automating tasks, particularly in integrating MISP with Microsoft Sentinel.
- Azure Monitor Agent: To collect security events from endpoints.
- Ubuntu: For the virtual machine running MISP in Docker.

## Steps Taken
1. Set up an Azure virtual machine (VM)
  ![Capture1](https://github.com/user-attachments/assets/0394bfec-ce40-4e5e-b90f-ceabf0e92e1b)

2. Installed and configured Windows Security Events on the VM to capture key security events.
   ![Capture2](https://github.com/user-attachments/assets/e9419c8e-bde7-4822-9657-0cd40da2b930)
   ![Capture3](https://github.com/user-attachments/assets/d77c143a-2d33-43af-aae4-e46fa1debb02)

3. Created a rule that checks for successful sign ins via RDP
  ![Capture10](https://github.com/user-attachments/assets/56bfa782-84ed-4306-8bbd-07aa5a6a14d0)
  ![Capture15](https://github.com/user-attachments/assets/8dbebe69-07a6-44da-983e-a0b8f05168e9)

4. Deployed MISP in a Docker container running on the Ubuntu VM.
![Capture16](https://github.com/user-attachments/assets/6ac7451c-222c-4b8d-a1d2-dfe544de43fa)

5. Followed the necessary steps to clone the MISP Docker repository and configure the container for web access.

![Capture22](https://github.com/user-attachments/assets/8b081b13-ceec-49e6-b103-52cb9f2a37b8)

6. Ran into an error where I realized I needed to not copy and paste in entirety and paste in sections.
![Capture18](https://github.com/user-attachments/assets/9e88e6d8-316f-440b-a3d4-232fd1b57a1e)

7. Cloned MISP-Docker Repo and double check.
![Capture23](https://github.com/user-attachments/assets/05d9004d-4748-4f81-9ae8-9b600b2b4759)

8. Customized .env file and change BASE_URL to point to public IP.
![Capture24](https://github.com/user-attachments/assets/6e3677c4-58c1-4ff8-b317-6571b17be49b)

9. Created a new port rule to open port 443 for HTTPS access to the MISP instance so I can access on different devices.
![Capture26](https://github.com/user-attachments/assets/dd1bdcfd-48c3-4743-8fb4-f3ffb0b1666c)

10. Utilized the json file from MISP repo for a set of MISP feeds to be enabled
![Capture28](https://github.com/user-attachments/assets/6b36806b-738b-4a38-9081-5858a6eb16c4)

This was done to enable threat indicators to be pulled from the threat inidicator sources. If you were to look at the json file itself, is that it is pulling URLs and giving other variables to be able to pull those indicators into the MISP instance.
![Capture30](https://github.com/user-attachments/assets/1c059e00-cf5f-42e2-b697-512341aa6f7c)

12. Set up data connector in the sentinal to pull threat indicators in.
![Capture31](https://github.com/user-attachments/assets/12260d15-2851-4182-b65f-20db97079d75)


14. Continue to follow MISP to Microsoft Sentinel Integration instructions and skip Keyvault step due to error.
![image](https://github.com/user-attachments/assets/32c25666-a3fb-4154-afbc-37c52a225635)

Downloaded repo to visual studio code. The Python script for a main function was not working and generated errors due to not having a multi tenant set up and deleted the following code.

![image](https://github.com/user-attachments/assets/bbbe47b7-227a-48b9-b308-919c37337d60)

16. Use the Function App to pull directly from MISP into Sentinel due to error. Decided to utlize environmental variables wihtin the Azure function app.
![image](https://github.com/user-attachments/assets/4cab3ff1-1b30-4bd0-a42d-28fdbe710aaf)

Ensure that the name of the function is the same as the config below: "tenant_id", "client_id", "client_secret" etc...

![image](https://github.com/user-attachments/assets/21cd05de-d3f9-4f87-a3f3-eb4ab6064baf)

18. Added a timer trigger schedule utliizing python script.

19. After completed, utilized Threat inteligence indicator to generate alerts.
![image](https://github.com/user-attachments/assets/6c10035e-70af-45af-8a1d-a48c6c0e582c)
![image](https://github.com/user-attachments/assets/12d986e7-a9c4-437d-858b-4205c4d43b09)

I had threat intelligence indicators, specifically network IP addresses, and I configured the system to flag any security event containing an IP address from a predefined list. For instance, if an event included an IP address from a list I created—composed of highly malicious IPs with a confidence score of 100—I integrated this into an analytics rule. This setup allowed me to establish a custom detection mechanism to identify these high-risk network IPs.

