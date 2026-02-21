Azure Mini Project 1 - Infrastructure as Code Web VM Deployment

Overview

This project aims to demonstrate the deployment of Azure infrasttructure using Infrastructure-as-Code (IaC) principles with Bicep, 
combined with automated server configuration using cloud-init.

The goal of this project was to build a reproducible Azure environment that automatically provisions a Linux Virtual Machine and deploys a static website from a public GitHub repository without any manual server configuration.

This project represents the foundation stage of a larger end-to-end Azure data platform that will later expand into Azure Storage, Azure SQL, Data Factory pipelines, and Power BI analytics.


Architecture

The deployed environment includes:

- Azure Resource Group
- Virtual Network and Subnet
- Network Security Group (Allow SSH and HTTP)
- Static Standard Public IP
- Network Interface
- Ubuntu Linux Virtual Machine
- NGINX Web Server
- cloud-init automation

Deployment Flow:

Internet -> Public IP -> NIC -> VM -> NGINX Website
Subnet -> VNet -> NSG Security Rules


Key Features of the Implementation:

- Infrastructure as Code (Bicep)
  - Declarative deployment of the Azure resources
  - Paramaterised SSH key authentication
  - Incremental redeployment workflow
  - CustomData integration for cloud-init automation process

- Automated Provisioning (cloud-init)
  - Installs nginx and git automatically
  - Clones a static website from Github
  - Deploys files into "/var/www/html"
  - Removes the default nginx web page
  - Starts and configures the web server on the first boot

- Security Configuration
  - SSH public key authentication (no passwords used)
  - Network Security Group rules for ports 22 and 80
  - Standard SKU Static Public IP


Technologies Used:

- Microsoft Azure
- Azure CLI
- Bicep
- Ubuntu Linux
- cloud-init
- NGINX
- Github

Deployment Instructions:

1. Generate an SSH Key (if one is not already present)

    ssh-keygen -t rsa -b 4096

  - This creates a public key at:
    
    #/.ssh/id_rsa.pub

  - The public key is passed into the Bicep deployment to enable secure VM access

2. Deployment Infrastructure

    az deployment group create \ 
     --resource-group <resource-group-name> \ 
     --template-file main.bicep \ 
     --parameters adminSshPublicKey="$(cat ~/.ssh/id_rsa.pub)"

3. Retrieve Public IP

    az network public-ip show \ 
     --resource-group <resource-group-name> \ 
     --name mini-webvm-pip \ 
     --query ipAddress \ -o tsv

4. Access Website

  - Open in a browser:

    http://<public-ip-address>

  - The VM will automatically deploy the Boostrap landing page via cloud-init


Project Outcome:

  - This project demonstrates

    - Azure infrastructure deployment using a Bicep file
    - Automated server configuration with cloud-init
    - Github-based static site deployment
    - Secure VM setup using SSH keys and NSG rules

Learning Objectives Achieved:

  - Understanding Azure networking and resource dependencies
  - Implementing Infrastructure-as-Code workflows
  - using cloud-init for automated provisioning
  - Applying secure authentication practices
  - Debugging deployment behaviour through iterative redeployment


Next Steps:

- This project is the first stage of my larger Azure learning pathway

 - Future planned enhancements:

   - Azure Stoarage Account and Azure SQL Database integration
   - Data modelling concepts
   - Azure Data Factory pipelines for automated ingestion
   - Power BI dashboards for analytics and visualisation

Made By: Jonathan Cormack