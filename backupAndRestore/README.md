RnD API Management : Back up and restore of API Gateway assets  

1.  [RnD API Management](index.html)

RnD API Management : Back up and restore of API Gateway assets
==============================================================

Created by Raj, Gokul, last modified by Raghu, Sreesaran on Nov 29, 2018

#### Author: 

Sreesaran Raghu

#### Supported Versions :

APIGateway 10.0 and above

Overview of the tutorial
========================

This tutorial explains the various steps for taking back up and restoring API Gateway assets (this includes all design time & run time assets) in case of disaster or failover of API Gateway in an unexpected time.

We will cover the following categories of possible locations where the data can be taken for back up.

1.  Local file system
2.  Network file system
3.  Cloud (Amazon s3)

Required knowledge
==================

In case of having backups saved in cloud the reader has to have the knowledge on creating Amazon S3 buckets.

In case of taking backups in cluster, the reader needs knowledge on making a file system shareable, so that it would be available across the network without any permission restrictions.

Why?
====

It is a good and necessary practice for the end consumers to take periodical backups of API Gateway data which helps the user to revert back to a desirable state of API Gateway in case of disaster or a failover. In case of not having a periodical backup, the customer might end up loosing  his accumulated data.

Details
=======

API Gateway ships with a script file using which the user can take a backup in a hassle free way. The script file for running back up command is _**apigatewayUtil.bat**_ which is located at _**<InstallationLocation>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\cli\\bin**_.The following sections provide a detailed steps on taking backups in different file systems in both standalone and cluster modes.

Note

APl Gateway should be running during the backup and restore process. Once the restoration is done, the server has to be restarted.

  

Taking backup in a standalone API Gateway
-----------------------------------------

Taking the back up in API Gateway is a two step process 

1.  Configure the repository where the backup will be stored. This is an optional step. In case, this step is not performed, API Gateway stores the back up in <INSTALL\_DIR>/InternalDataStore/archives 
2.  Run the backup script. 

### Backup in Local file system

#### Configure backup location

**Configure custom File System (optional)**

apigatewayUtil.bat configure fs\_path  -path <Backup\_Location>                         -> Configure File path

Restart APIGateway 
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin                     -> Navigate to Directory
shutdown.bat                                                                          -> shutdown of API Gateway                                               
startup.bat                                                                           -> startup of API Gateway

Note : If Elasticsearch autostart is disabled, then Elasticsearch needs to be restarted along with API Gateway

#### Run the backup script

Follow the below steps to take back up in a local file system.

**Backup in standalone mode using Local file system**

apigatewayUtil.bat create backup  -name <BackupName>                                   -> Create Backup


# Verify the back up status
apigatewayUtil.bat status backup  -name <BackupName>                                   -> Verify the state of  created backup
Note: The backup is successful, only if the status is returned as SUCCESS

#### Restore data in API Gateway

You can restore the backed up data anytime by using the following command 

**Restore in standalone mode using Local file system**

apigatewayUtil.bat restore backup -name <BackupName>                                  -> To Restore the Backup

After a successful restore of Data, restart API Gateway
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin                     -> Navigate to Directory
shutdown.bat                                                                          -> shutdown of API Gateway                                               
startup.bat                                                                           -> startup of API Gateway

### Backing in Network file system

This section explains taking back up in a network file system.

Note

Backup of API Gateway should be performed while the file system is available in the network with full control permission for the user who creates the backup.

Example File in Network  → //<machine>/apigatewayBackup

#### Configure backup location

**Configure File system**

apigatewayUtil.bat configure fs\_path  -path <Network Location>               -> Configure File path

After configuring networked file system a restart of APIGateway is required
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin            -> Navigate to Directory
shutdown.bat                                                                 -> shut down of API Gateway
startup.bat                                                                  -> startup of API Gateway

Note : If Elasticsearch Autostart is disabled, then Elasticsearch needs to be restarted along with APIGateway 

#### Run the backup script

Follow the below steps to take back up.

**Backup in standalone mode using Local file system**

apigatewayUtil.bat create backup  -name <BackupName>                                   -> Create Backup
apigatewayUtil.bat status backup  -name <BackupName>                                   -> Verify the state of  created backup
Note: The backup is successful, only if the status is returned as SUCCESS

#### Restore data in API Gateway

You can restore the backed up data anytime by using the following command

**Restore in standalone mode using Local file system**

apigatewayUtil.bat restore backup -name <BackupName>                                  -> To Restore the Backup

After a successful restore of Data, restart API Gateway
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin                     -> Navigate to Directory
shutdown.bat                                                                          -> shutdown of API Gateway                                               
startup.bat                                                                           -> startup of API Gateway

### Using Cloud storage - Amazon S3

This section explains taking back up in a cloud storage file system.

Steps for taking back up data in Amazon S3 are as follows.

*   Install Amazon s3 plugin in Elasticsearch
*   After successful install of plugin, a restart of Elasticsearch must be done
*   The cloud configuration file "gateway-s3-repo.cnf" must be configured with details of the Amazon s3 bucket

#### Steps to Install Amazon S3 plugin in EventDataStore Or Elasticsearch

##### For API Gateway Version 10.0 & 10.1

**Backup And Restore in Standalone Instance(using Cloud Storage)**

cd <InstallationLocation>\\EventDataStore\\bin\\                                -> Navigate to Directory
plugin.bat install cloud-aws
 
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin            -> Navigate to Directory
shutdown.bat                                                                 -> shut down of API Gateway
startup.bat                                                                  -> startup of API Gateway

##### For APIGateway Version 10.2 & above

**Backup And Restore in Standalone Instance(using Cloud Storage)**

cd <InstallationLocation>\\EventDataStore\\bin\\                                -> Navigate to Directory
elasticsearch-plugin.bat install repository-s3

cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin            -> Navigate to Directory 
shutdown.bat                                                                 -> shut down of API Gateway
startup.bat                                                                  -> startup of API Gateway

#### Configuration File example

**gateway-s3-repo.cnf  
**

type=s3
bucket=<s3-bucket-name>
region=<s3-region>
access\_key=<s3-access-key>
secret\_key=<s3-secret-key>
base\_path=<s3-base-path>

  

**Example**

type=s3
bucket=s3 bucket
region=ap-south-1
access\_key=apikey
secret\_key=secretKey
base\_path=

  

#### Configure Amazon S3 Repo

Steps to configure the S3 bucket details in API Gateway

**Configure S3 bucket details**

apigatewayUtil.bat configure manageRepo -file <Location of CloudFileStorageConfiguration.cnf>    -> configure file

#### Run the backup script

Follow the below steps to take back up in a cloud storage system.

**Backup in Standalone Instance(using Cloud Storage)**

apigatewayUtil.bat create backup -name <BackupName>                                              -> Create Backup
apigatewayUtil.bat status backup -name <BackupName>                                              -> Verify the state of  created backup

#### Restore data in API Gateway

Follow the below steps to restore data in API Gateway

**Restore in Standalone Instance(using Cloud Storage)**

apigatewayUtil.bat restore backup  -name <BackupName>                                            -> To Restore the Backup

cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin                                -> Navigate to Directory 
shutdown.bat                                                                                     -> shut down of API Gateway
startup.bat                                                                                      -> startup of API Gateway

Backup in API Gateway cluster
-----------------------------

  

Note

Networked file system used for saving API Gateway backups should have Full file permission.  

### Using Network file system

The below section provides the steps for taking back up for a clustered API Gateway in a Network file system.

#### Configure Backup location

Follow the below steps to take back up in a cloud storage system for an API Gateway cluster.

**Configure Networked Location**

API Gateway 1-> Node1, API Gateway 2-> Node2
 
Configure Network Location in both the Nodes
apigatewayUtil.bat configure fs\_path  -path <Network Location>                         -> Configure File path

shutdown & restart both API Gateway's
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin            -> Navigate to Directory 
shutdown.bat                                                                 -> shut down of API Gateway
startup.bat                                                                  -> startup of API Gateway 

#### Run the backup script

Follow the below steps to take back up.

**Backup data in Clustered Instance(using Shared File System)**

API Gateway 1-> Node1, API Gateway 2-> Node2
 
Backup can be initiated from either Node 1 or Node 2 
apigatewayUtil.bat create backup -name <BackupName>                                    -> Create Backup

  
apigatewayUtil.bat status backup -name <BackupName>                                    -> Verify the state of  created backup
Note: only if the the status is returned as SUCCESS, this can be done from any of the nodes

#### Restore data in API Gateway

Follow the below steps to restore data in API Gateway

**Restore in Clustered Instance(using Shared File System)**

Restore can be done from either Node 1 or Node 2 
apigatewayUtil.bat restore backup -name <BackupName>                                   -> To Restore the Backup

Applicable for Both Nodes
cd <InstallationLocation>\\IntegrationServer\\instances\\default\\bin                      -> Navigate to Directory
shutdown.bat                                                                           -> start API Gateway
startup.bat                                                                            -> stop API Gateway

  

### Using Cloud storage - Amazon S3

This section explains taking back up in a cloud storage file system.

The Amazon S3 plugin for Elasticsearch needs to be installed in both Nodes and the steps for installing the plugin are mentioned above.

After which the backup can be initiated from any one or clustered Nodes.

Backup and restore support matrix for standalone and cluster modes in different file systems
--------------------------------------------------------------------------------------------

Note

 This table is a synopsis of supported File system against Standalone APIGateway & Clustered APIGateway 

APIGateway Configuration

Local file system

Network file system

cloud

standalone mode

![](images/icons/emoticons/check.png)

![](images/icons/emoticons/check.png)

![](images/icons/emoticons/check.png)

cluster mode

![cross (x)](images/icons/emoticons/error.png)

![](images/icons/emoticons/check.png)

![](images/icons/emoticons/check.png)

Troubleshooting
===============

**TroubleShooting Tips**

Tips for Trouble Shooting :
 
including option "-debug true" along with the command, will list all the error logs.
 
Example :- 
apigatewayUtil.bat create backup -tenant <tenantName> -name <BackupName> -debug true
 
To list all available commands
apigatewayUtil.bat -help

Limitations
===========

*   This back up and restore utility supports only the backup of API Gateway assets which are available in Elasticsearch and is not applicable for any other data or configurations that are available in Integration server

FAQ
===

Recommendation

Q: Should APlGateway be running be during the backup process ?  
A: API Gateway should be running during backup

Q: After the Restore of Data in API Gateway, is a complusary restart is required ?  
A: Yes it is required.

Q: Why do need to Install Amazon s3 plugin when backing up the data to cloud ?  
A: Amazon s3 plugin is not supported out of the box and hence it needs to installed.

Q: Will my backup include the data, that has been generated while backup process is in progress ?  
A: No it will not. Dont worry it will not get lost, it will get included in the next backup process.

Q: What special characters are allowed in backup name and is there any other restriction ?  
A: The characters "!" , "@" , "$" , "%" , "( , )" are allowed and also backups cannot contain any **Capital Letters**.

Q: Restoring a backup was unsucessful, how do i know the reason for the failure ?  
A: Run the command again with **\-debug true**.

Q:Running the Restore command with **\-debug true**, show the error "**IndexNotFoundException\[no such index\]**" message what does it mean?  
A:Backup files got tampered and Restoration of data will not be possible.

Q: Can I migrate my data using this backup and restore ?  
A: No that is not supported using backup and restore.

  

  

  

Document generated by Confluence on Apr 04, 2020 11:34

[Atlassian](http://www.atlassian.com/)