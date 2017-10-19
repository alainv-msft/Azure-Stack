ConfigASDK.ps1 (updated for asdk build 20170928.3)
==============
Description
-----------

The purpose of this script is to automate as much as possible post deployment tasks in Azure Stack Development Kit
This include :
* Set password expiration
* Disable windows update on all infrastructures VMs and ASDK host
* Tools installation (git, azstools, Azure Stack PS module)
* Windows Server 2016 and Ubuntu 16.04-LTS images installation
* MySQL Resource Provider Installation
* SQL Resource Provider Installation
* Deployment of a MySQL 5.7 hosting Server on Windows Server 2016 Core
* Deployment of a SQL 2014 hosting server on Windows 2016
* AppService prerequisites deployments (fileserver and sqlserver vms)
* AppService Resource Provider sources download to c:\Temp\appservice and certificate generations
* Set new default Quotas for Compute, Network, Storage and keyvault
* Create a simple offer and plan to provide IaaS capabilities to tenants

Instructions
------------

* Login as your service adminstrator on your ASDK host.
* Edit the script and set the proper value to the following variables :
	
		$ISOPath = "PATH_TO_WIN2016_ISO"                             # path to your windows 2016 evaluation ISO
		$rppassword = "ADMINPASSWORD_FOR_RP_INSTALLATION" 			 # Resource Providers administrator account on all machines deployed for resource providers
		$azureDirectoryTenantName = "YOUR_AAD_TENANT_NAME"           # your Azure Tenant Directory Name for Azure Stack if AAD
	
* Open an elevated powershell window and run the script using -AAD parameter if you are using Azure AD otherwise the script will assume this is an ADFS deployment. 
* You will be prompted for credentials, these are your azure stack service administrator credential then your azurestack\azurestackadmin credentials
* mysqlrp and sqlrp administrator account will be "cloudadmin". These logins are also applicable for hosting servers.
* fileserver vm for appservice will use fileshareowner as administrator account
* the password set for every login is the one set for the $rppassword variable in the script

Post script actions
-------------------	
This script can take up to 5 hours to finish.
Once the script is finished you have to complete the following:

* AppService installation you need to continue from the Create AAD application step from here : https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-app-service-before-you-get-started
* You need to attach your capacity hosts (sql and mysql) to their resource providers from the admin portal
* You have to register your system if you want to enable marketplace syndication. follow these steps https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-register
* Create your plans to offer services to tenants


Usage Example:
-------------

	ConfigASDK.ps1 -AAD 