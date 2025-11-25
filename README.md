# CTI-Integration Home Lab

## Objective

Creating the CentralLight Open Source Threat Intelligence Home Lab for easy access and utilization of open‑source threat intelligence tools.

## Skills Learned 
- Deployed Ubuntu server and integrated it with OpenCTI and Splunk for centralized threat intelligence management.
- Used GitHub to manage code and integrated it into Docker Compose (YAML) for deployment.
- Connecting and integrating AlienVault OTX with security platforms via API.

## Tools Used
- OpenCTI - Threat Intelligence Platform
- Docker - for packaging, distributing, and running applications in isolated environments
- Splunk - (SIEM)
- Github
- AlienVault OTX - Platform to collect and share threat intelligence data.
- UUID Generator
  
### Note

- I use Ubuntu Server version 22.04.5 For the OpenCTI server, I allocated 4GB of RAM and 100GB of storage.

## Set Up the OpenCTI

After installing Ubuntu Server in a virtual machine, I established an SSH connection using PowerShell. This setup makes it easier to copy and paste commands. And I installed OpenCTI on using docker.

Go to https://docs.opencti.io/latest/deployment/installation/#using-docker. And Follow the instruction. 
Run these commands  **sudo apt install docker-compose**
and  **git clone https://github.com/OpenCTI-Platform/docker.git**

Go to the docker directory and run ls -al . You will see the docker-compose.ymal file, which contains all of the configurations that Docker will reference. This YAML file also references the environment variables defined in **.evn.sample**.  The docker-compose.yml file and pulls variables from .env.sample  file, so we need to make modifications there. And Change that file name to **.env**.

We will use the mv command on .env.sample to rename it to .env . After that, we will modify the  file as needed.

<img width="710" height="387" alt="Screenshot 2025-11-24 234044" src="https://github.com/user-attachments/assets/85739dca-6b77-4b02-9c28-bc1252d70065" />

As referenced in the image, make the following modifications 

Generate a UUID from https://www.uuidgenerator.net/ and use it as the value for **.OPEN_ADMIN_TOKEN**
Other *_PASSWORD variables set these to a password of your choice. For example, I assigned:** P@ssw0rd!**

Now that we have updated our environment variables, the final step is to configure Elasticsearch. This involves assigning a static **vm.max_map_count** value, which helps with memory management. : **sudo sysctl -w vm.max_map_count=1048575**

**With all the configurations complete, we can now start Docker**

: **sudo docker-compose up -d**

Now you can access OpenCTI in your web browser.
Use the host IP address you assigned along with port 8080.
For the username and password, use the values of OPENCTI_ADMIN_EMAIL and OPENCTI_ADMIN_PASSWORD from the .env file.

<img width="1904" height="883" alt="image" src="https://github.com/user-attachments/assets/6239735e-b263-4636-98f6-207b4adff480" />

## Thrent Intelligence Feed ingestion

After the installation, I will ingest the Alivant Vault Threat Intelligence feed. Go to the OpenCTI External Connectors -> https://github.com/OpenCTI-Platform/connectors/tree/master/external-import . And look for the Open CTI Alien Vault Connector. And check the docker-compose.yml file to use this format as an example. Then Update it to our docker-compose.yml. You need to have Alienvault account to obtain Alivent Vault API Key. 

Fill in the necessary information as highlighted:

<img width="817" height="559" alt="Screenshot 2025-11-25 173933" src="https://github.com/user-attachments/assets/1023eca4-9f75-4ddb-843a-fbf1322da116" />

**OPENCTI_TOKEN**
Use the same key as your previously generated . **OPEN_ADMIN_TOKEN**

**CONNECTOR_ID**
Generate a new UUID from https://www.uuidgenerator.net/.

**ALIENVAULT_API_KEY**
Obtain this key from https://otx.alienvault.com/api.
You will need an AlienVault account to access the API key.

Now you can access the AlienVault Threat Intelligence feed on OpenCTI.

<img width="1886" height="885" alt="Screenshot 2025-11-25 011701" src="https://github.com/user-attachments/assets/de2a3a75-814f-4cbc-86d8-8478168f3275" />

 ###### Note
- To check whether the AlienVault ingestion was successful, go to the Ingestion section under the Data and Monitoring tab on the right

## Summary 
OpenCTI is a powerful tool, and if you utilize and adapt it properly, it can be very useful for your necessary cases.


