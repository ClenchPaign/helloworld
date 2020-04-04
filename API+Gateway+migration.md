API Gateway migration steps - Simplified
========================================

#### Author: [Madharsha, Sadiq](/wiki/pages/createpage.action?spaceKey=CLEP&title=%2Fdisplay%2F~SADM&linkCreation=true&fromPageId=557058)

#### Supported Versions:

API Gateway 10.3 Fix 4 and above

Overview of the tutorial
========================

API Gateway supports migration of data from older version to newer versions. In this tutorial we will go through the following migration types in detail.

*   Migration using direct mode for standalone
*   Migration using direct mode for cluster
*   Migration using backup mode for standalone
*   Migration using backup mode for cluster

Required knowledge
==================

The tutorial assumes that the reader has,

*   a basic knowledge on the API Gateway as a product
*   a basic understanding on Elasticsearch (Internal Data Store)

Why?
====

In earlier versions of API Gateway i.e before 10.3 Fix 4 the migration commands were more complex which involves a multi step process. A lot of manual steps were needed to perform the migration and adding an overhead of restarting API Gateway server and Elasticsearch multiple times. In addition to that the user has to face few more struggles, some of them are, the migration didn't support migration of data from externally configured Elasticsearch and for any troubleshooting, the user has to look out multiple locations for the logs instead of single unified location which is far more easier.  
The new migration utility introduced in API Gateway 10.3 Fix 4 will resolve the below issues.

*   The commands are very simple and few
*   Restart of Elasticsearch and API Gateway server is eliminated
*   Migration of Elasticsearch and IS can be done separately
*   Migration of data from externally configured Elasticsearch is supported
*   Logs all the details to a standard file, _migrationLog.txt_, a single file for all the log data
*   Supports reverting in case of failure in Elasticsearch migration

Prerequisite steps
==================

Complete the below prerequisites before working on migration.

*   Install source and target API Gateway instances. The version of target API Gateway should be higher than source API Gateway. Supported source API Gateway versions are 10.1 and above
*   If custom keystore files are used in the source API Gateway installation, copy the files to the same location in the target installation
*   If the target API Gateway is 10.5, then apply Fix 1

Details
=======

The migration of API Gateway can be done in two ways.

#### 1. Direct  mode

This is done by running the migration utility by referring to source Elasticsearch connection properties for migrating Elasticsearch data and the source API Gateway installation directory for migrating the IS configuration data. If the source and target Elasticsearch instances are running in the same network and can talk to each other then this method is preferred.

#### 2\. Backup mode

This is done by running the migration utility by referring to the backup of source Elasticsearch data and the source API Gateway configuration backup file for migrating both the Elasticsearch data and the IS configuration. If the source and target Elasticsearch instances are running in different network and not able to talk to each other then this method is preferred.  
Throughout this tutorial we refer source API Gateway installation directory as **<SOURCE>,** target API Gateway installation directory as **<TARGET>** and target elasticsearch is **<TARGET\_ELASTIC\_SEARCH>**  
Note  
In API Gateway 10.2 and above the folder name "EventDataStore" has been changed to "InternalDataStore".

Migration using direct mode for standalone
------------------------------------------

In this scenario we will be migrating a standalone API Gateway to a higher version using direct mode. The steps are given below.

### Step 1

Configure the below property in the target elasticsearch.yml file located at **_<TARGET>\\InternalDataStore\\config_** for re-indexing the data in the target Elasticsearch. The below property helps to copy the documents from **<SOURCE>** to **<TARGET>** Elasticsearch instance.  
**elasticsearch.yml**  
reindex.remote.whitelist: <source\_host>:<source\_port>  
(This value should match with the value of the property pg.gateway.elasticsearch.hosts present in  
<SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resource\\elasticsearch\\config.properties)  
For example, the below file contains _localhost:9240_ for _reindex.remote.whitelist_ property.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav68714fddb4f68854a66e2e359d2b0b94.png?api=v2)

### Step 2 (Optional)

This step is applicable only if the source API Gateway version is 10.1.  From 10.2  these values are populated automatically.  
Configure source Elasticsearch host and port details in the file **_config.properties_** file which is located at **_<SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch_**.  
**config.properties**  
pg.gateway.elasticsearch.hosts=<source\_host>:<source\_port>

### Step 3

If the source Elasticsearch is protected with basic authentication add the below properties in **_<SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch\\config.properties_** file.  
**config.properties**  
pg.gateway.elasticsearch.http.username=<username>  
pg.gateway.elasticsearch.http.password=<password>

### Step 4

If the source Elasticsearch is protected with HTTPS, add the source certificates (public key) in to the target Elasticsearch JVM's truststore. For e.g, In case of internal data store we need to add the public keys to the truststore cacerts file located at **_<TARGET>\\jvm\\jvm\\jre\\lib\\security_**.  
Note  
If external Elasticsearch is used for the target API Gateway then the certificates should be imported to its corresponding JVM  
Below is a sample command to import the truststore of source Elasticsearch in to target Elasticsearch JVM.  
**_$<TARGET>\\jvm\\jvm\\bin\\keytool -import -keystore <TARGET>\\jvm\\jvm\\jre\\lib\\security\\cacerts -file <truststore.jks>-alias <alias>_**

|     |     |     |
| --- | --- | --- |
| **Property** | **Detail** | **Example** |
| <truststore.jks> | Truststore used in <SOURCE> Elasticsearch. Provide the full path of the truststore.  <br>  <br>For 10.1 it should be available at _C:\\installation\\source\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\bean\\gateway-es-store.xml_  <br>property **_<prop key="searchguard.ssl.http.truststore\_filepath">sagconfig/root-ca.jks</prop>_**  <br>  <br>For 10.2 and above it will be available at _C:\\installation\\source\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch\\config.properties_  <br>property **_pg.gateway.elasticsearch.https.truststore.filepath_** | _sagconfig/root-ca.jks_ |
| <alias> | This is the alias used in <SOURCE> Elasticsearch | wm.sag.com |

### Step 5

Start both source and target Elasticsearch instances and make sure that IS instances are NOT started. Also avoid Elasticsearch port conflict.  
Avoid port conflict  
If source and target API Gateway instances are running in the same machine, then the user might not able to start both the source and target Elasticsearch instances in parallel with the default port configurations. In that case the target Elasticsearch instance port can be changed temporarily for running the migration. Both HTTP and TCP ports must be changed. Follow the below steps.

1.  Go to _<TARGET>/InternalDataStore/config directory_, open the _elasticsearch.yml_ file, and change the value of the HTTP port in the _http.port_ property and the TCP port in the _transport.tcp.port_ and _discovery.zen.ping.unicast.hosts_ properties
2.  Go to _<TARGET>/IntegrationServer/instances/default/packages/WmAPIGateway/config/resources/elasticsearch_ directory, open the _config.properties_ file and find the _pg.gateway.elasticsearch.hosts_ property. If the property is set to _changeOnInstall_ then you need to do nothing further. If there is a port configured already, update it to a new value

### Step 6

Now we will start running the migration process. The migration process consist of migration of Elasticsearch data and API Gateway configurations and both will be done by running the migration utility command at <TARGET> API Gateway machine.  
In this step we will migrate the Elasticsearch data.   
Go to _<TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_ and run the below command.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavc7cd2bbb3609544f28e4e90fe8c002dc.png?api=v2)  
**_$> migrate.bat datastore -dstoreSrc <full path to source Elasticsearch config.properties>_**

|     |     |
| --- | --- |
| **Parameter** | **Description** |
| dstoreSrc | If source and target API Gateway instances are running in the same network. Provide the location where <SOURCE> config.properties file is located.  <br>Sample:  <br>_migrate.bat datastore -dstoreSrc  <SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch\\config.properties_ |
|     | If the source and target instances are running in different machines, the the source installation directory or at least the Elasticsearch _config.properties_ file must be shared in the network. Otherwise just copy and paste the source config.properties to the shared location.  <br>Sample:  <br>_migrate.bat datastore -dstoreSrc  \\\\chebackup01\\installations\\source\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch\\config.properties_ |

**_$> migrate.bat datastore -dstoreSrc  \\\\chebackup01\\installations\\source\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch\\config.properties_**  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavf6ac95393ab7efa5e08fcd91fea68dea.png?api=v2)

### Step 7

In this step we will migrate the Integration Server's configuration to target instance. Go to _<TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_ and run the below command.  
**_$> migrate.bat apigateway -srcDir <SOURCE> -instanceName <source instance name> -newInstanceName <target instance name>_**  

|     |     |     |
| --- | --- | --- |
| **Parameter** | **Description** | **Sample command** |
| srcDir | If the source API Gateway instance is installed in the same network, provide the source API Gateway installation directory | _\-srcDir  C:\\installations\\source_ |
|     | If the source API Gateway instance is installed in a different network, then share the entire installation folder | _\-srcDir \\\\chebackup01\\source_ |
| instanceName | This is an optional parameter. Here we need to pass <SOURCE> instance name.  <br>If you don't provide any name then _default_ will be assigned. If you want to migrate different instance other  <br>than _default_ provide its name. (To know more about Integration server instances please refer its doc) | _\-instanceName  default_  <br>_\-instanceName  dev_  <br>_\-instanceName  test_ |
| newInstanceName | This is an optional parameter. Here we need to pass <TARGET> instance name.  <br>If you don't provide any name then _default_ will be assigned. If you have created a new instance other than _default_  <br>in Integration server and you want to migrate to the new instance then provide its name. | _\-newInstanceName default_  <br>_\-newInstanceName qa_  <br>_\-newInstanceName prod_ |

A sample run command is given below.  
**_$> migrate.bat  apigateway  -srcDir  C:\\installations\\source  -instanceName  default  -newInstanceName default_**  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav6a84ed01ebb010fced1d328d2b9e0185.png?api=v2)  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddave5f4d8c6a6b3700192f9342a6e92602a.png?api=v2)

### Step 8

This is a post migration step. Follow the below steps.

*   Shutdown all Elasticsearch instances
*   Start API Gateway server
*   You can find the logs  at target directory  **_<TARGET>/install/logs/migrationLog.txt_** 
*   You can find the API Gateway migration reports at  **_<TARGET>\\install\\logs\\APIGW\_Migration\_Reports_<date\_time>\_**

Migration using direct mode for cluster
---------------------------------------

This is very similar to the standalone migration except the following differences.

*   Go to <SOURCE>\\InternalDataStore\\config and configure **_path.repo_** property in elasticsearch.yml file for all the nodes. Make sure that the _path.repo_ is a shared network folder and should be accessible for all the Elasticsearch nodes in the cluster. 
*   Follow Step 1 all the target Elasticsearch instances in the cluster
*   Follow Step 6 in only one node in the cluster
*   Follow Step 7 for all API Gateway target nodes in the cluster

Before running the migration, setup the target Elasticsearch cluster and make sure that all the source and target Elasticsearch instances in the clusters are up and running with the exception that the API Gateway instances should not be started on any of the nodes.

Migration using backup mode for standalone
------------------------------------------

In this scenario we will be migrating a standalone API Gateway to a higher version using backup mode. The steps are given below.  
Target API Gateway is 10.5  
If the target API Gateway version is 10.5, this Backup mode is not supported to migrate Elasticsearch data due to the fact that the Elasticsearch version would be different in source API Gateway i.e less than 7.2.0. As per the Elasticsearch documentation, restoring a backup which is taken from a version less than 7.2.0 (for e.g 5.6.4), into version 7.2.0 is not supported. As a workaround to this, please follow these steps.

*   Install a temporary Elasticsearch of a version which is supported by the source API Gateway in a machine and make it accessible to the target API Gateway instance, i.e, the target Elasticsearch 7.2.0.
*   Start the temporary Elasticsearch.
*   Restore the <SOURCE> Elasticsearch backup into the temporarily running Elasticsearch by following the steps given in this section (_Migration using backup mode for standalone_).
*   After that, use the temporary Elasticsearch instance as the source Elasticsearch and migrate the Elasticsearch data to the <TARGET> API Gateway (10.5) using Direct mode followed by Step 7.

  
TARGET\_ELASTIC\_SEARCH  
In this section, if the <TARGET> version is 10.5, **_<TARGET\_ELASTIC\_SEARCH>_** refers to the location of the temporarily running Elasticsearch. For versions less than 10.5, **_<TARGET\_ELASTIC\_SEARCH>_** refers to <TARGET>/InternalDataStore.

### Step 1

Go to **_<SOURCE>\\InternalDataStore\\config_** and configure _path.repo_ property in _elasticsearch.yml_ file. **_path.repo_** is an elastic search property and it helps us to create a repository in the Elasticsearch where the backup data will be stored. This step would help preparing the <SOURCE> Elasticsearch to store the data in this location.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav3a4e43c10ff7ce638837008aa7f6f742.png?api=v2)

### Step 2

Start the <SOURCE> Elasticsearch instance.

### Step 3

Now lets take the backup of source API Gateway data. Go to **_<SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\cli\\bin_** and run the below command.  
**_$> apigw-upgrade-backup.bat -backupDestinationDirectory <path\_to\_data\_store\_backup\_folder>  -backupFileName <backup\_file\_name\_without\_spaces\_for\_the\_zip>_**  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavc2dc3391f3c23353f5675bb7ca0dc7c5.png?api=v2)

|     |     |     |
| --- | --- | --- |
| **Parameter** | **Description** | **Sample command** |
| _backupDestinationDirectory_ | Same network. The location where the cumulative backup data will get stored. Make sure  <br>the directory is already created before executing the command. | _\-backupDestinationDirectory C:\\migration\\backupFolder_ |
| _backupFileName_ | Backup file name. The name of the .zip file that will be created as part of the backup command  <br>execution. Make sure the backup file name is specified in lower case | _\-backupFileName backupzipfile_ |

Example,  
**_$> apigw-upgrade-backup.bat -backupDestinationDirectory C:\\migration\\backupfolder -backupFileName backupzipfile_**   
Note  
Make sure the backup destination directory is already created and the backup file name be specified in lower case.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav6669e8a329896b7aa677741e054f4c66.png?api=v2)  
After the command is run successfully, the backup directory would contain the Elasticsearch data folder which is stored with the tenant name (for our use case it is _default_) and API Gateway configuration data as a zip file (for our use case it is _backupzipfile.zip_) as seen in the below screenshot. The Elasticsearch data folder is copied from the location we configured in Step 1 by the command.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddava6036dac30a332cb36e452e4fb557fdb.png?api=v2)

### Step 4

1.  After the backup command is executed and the backup is done, stop the <SOURCE> Elasticsearch.  
2.  Now go to **_<TARGET\_ELASTIC\_SEARCH>\\config_** and configure _path.repo_ property in _elasticsearch.yml_ file and start the <TARGET> Elasticsearch instance.

### Step 5

When the **<TARGET\_ELASTIC\_SEARCH>** is up and running, invoke the below RESP API to create a repository in the <TARGET> machine with tenant name.  
PUT /\_snapshot/tenant\_name  
{ "type": "fs", "settings": { "location": <tenant\_name> } }  
For e.g if we want to create 'a repo for 'default' as tenant name, then the command would be as below.  
PUT   [http://localhost:9240/\_snapshot/default](http://localhost:9240/_snapshot/default)   
{ "type": "fs", "settings": { "location": "default" } }  
After the REST API invocation, a folder with the tenant name (in our use case it is _default_) will be created under the _path.repo_ folder.

### Step 6

This is a preparation step for Elasticsearch data migration. Go to **_backupDestinationDirectory/<tenant\_name>_** directory in Step 3 and copy the contents from Elasticsearch data folder (with tenant name) to the repository folder created in Step 5.  
For e.g the below is the Elasticsearch data folder for our use case.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddava100d7da79e8dcf3ce0a72fb8a284246.png?api=v2)  
The below is the target repository folder under _path.repo_.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavd9300c09cfc3f7f56ce06afe87038e01.png?api=v2)

### Step 7

Once the above prerequisites are ready we are now ready to run the migration commands to migrate the Elasticsearch data and the API Gateway configurations. In this step we will migrate the Elasticsearch data.

#### For <TARGET> API Gateway 10.5

Restore the snapshot using the following command in the target instance  
_curl -X POST "localhost:9240/\_snapshot/__<__tenant\_name>/<backupFileName>/\_restore"_  
For e.g if we want to create a repo with 'default' as tenant name, then the command would be as below.  
_POST_ {\_}[http://localhost:9240/\_snapshot/default/backupzipfile/\_restore\_](http://localhost:9240/_snapshot/default/backupzipfile/_restore_)  
As the restore command is asynchronous, you can check its status by using the below command  
\_[http://localhost:9240/\_cluster/health?pretty\_](http://localhost:9240/_cluster/health?pretty_)  
The restore would be completed successfully when the health status becomes yellow from red. Now use this temporary Elasticsearch as source Elasticsearch and migrate the Elasticsearch data by direct mode as mentioned in the section '_Migration using direct mode for standalone_'

#### For <TARGET> API Gateway below 10.5

Go to _<TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_ directory and run the below command.  
**_$> migrate.bat datastore_**  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav729c63d584846e0fdebaf25b21e1ae49.png?api=v2)  
After command is run the data store data will be migrated.

### Step 8

After successful migration of Elasticsearch data, run API Gateway configurations migration using the below command.  
**_$> migrate.bat apigateway -srcFile <backupLoc>  fileName.zip  -instanceName <old instance name> -newInstanceName <new instance name>_**

|     |     |     |
| --- | --- | --- |
| **Parameter** | **Description** | **Sample command** |
| _srcFile_ | Provide the .zip file location that were created as part of backup process. This can also be a shared network file. | _\-srcFile C:\\migration\\backupfolder\\backupzipfile.zip_ |
| _instanceName_ | This is optional parameter. Here we need to pass <SOURCE> instance name .  <br>If you don't provide any name then 'default' will be assigned. If you want to migrate different instance other  <br>than default, please provide its. (To know more about Integration server instances please refer its doc) | _\-instanceName default_   <br>_\-instanceName test_ |
| _newInstanceName_ | This is optional parameter. Here we need to pass <TARGET> instance name .  <br>If you don't provide any name then 'default' will be assigned. If you have created a new instance other than default  <br>in Integration server and you want to migrate to the new instance then please provide its name for -newInstanceName option | _\-newInstanceName default_  <br>_\-newInstanceName prod_ |

  
For e.g **_$> migrate.bat apigateway -srcFile C:\\migration\\backupfolder\\backupzipfile.zip -instanceName default -newInstanceName default_**  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavb6d98b011c8caef8f7b3ed13b0a65c24.png?api=v2)  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddavb77a6927522b7ada0698573a8578631d.png?api=v2)  
Now the API Gateway configurations are migrated.

### Step 9

This is a post migration step. Follow the below steps.

*   Shutdown the <TARGET> Elasticsearch instance
*   Start the <TARGET> API Gateway server

Migration using backup mode for cluster
---------------------------------------

In this scenario we will be migrating a clustered API Gateway to a higher version using backup mode. This is very similar to the standalone migration except few differences. The steps are given below.  
Target API Gateway is 10.5  
If the target API Gateway version is 10.5, this Backup mode is not supported to migrate Elasticsearch data due to the fact that the Elasticsearch version would be different in source API Gateway i.e less than 7.2.0. As per the Elasticsearch documentation, restoring a backup which is taken from a version less than 7.2.0 (for e.g 5.6.4), into version 7.2.0 is not supported. As a workaround to this, please follow these steps.

*   Install a temporary Elasticsearch of a version which is supported by the source API Gateway in a machine and make it accessible to the target API Gateway instance, i.e, the target Elasticsearch 7.2.0.
*   Start the temporary Elasticsearch.
*   Restore the <SOURCE> Elasticsearch backup into the temporarily running Elasticsearch by following the steps given in this section (_Migration using backup mode for cluster_).
*   After that, use the temporary Elasticsearch instance as the source Elasticsearch and migrate the Elasticsearch data to the <TARGET> API Gateway (10.5) using Direct mode followed by Step 8.

  
TARGET\_ELASTIC\_SEARCH  
In this section, if the <TARGET> version is 10.5, **_<TARGET\_ELASTIC\_SEARCH>_** refers to the location of the temporarily running Elasticsearch. For versions less than 10.5, **_<TARGET\_ELASTIC\_SEARCH>_** refers to <TARGET>/InternalDataStore.

### Step 1

Before running the migration, setup the target Elasticsearch cluster and make sure that all the source and target Elasticsearch instances in the clusters are up and running with the exception that the API Gateway instances should not be started on any of the nodes.

### Step 2

Go to **_<SOURCE>\\InternalDataStore\\config_** and configure _path.repo_ property in _elasticsearch.yml_ file. Do this for all the nodes. Note that Note that _path.repo_ should be a shared network folder and should be accessible for all the Elasticsearch nodes in the cluster. 

### Step 3

Start all the <SOURCE> Elasticsearch instances.

### Step 4

Now lets take the backup of source API Gateway data. Go to any one of the Elasticsearch nodes location**_<SOURCE>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\cli\\bin_** and run the below command.  
**_$> apigw-upgrade-backup.bat -backupDestinationDirectory <path\_to\_data\_store\_backup\_folder>  -backupFileName <backup\_file\_name\_without\_spaces\_for\_the\_zip>_**  
For Example,  
**_$> apigw-upgrade-backup.bat -backupDestinationDirectory C:\\migration\\backupfolder -backupFileName backupzipfile_**   
Note  
Make sure the backup destination directory is already created and the backup file name be specified in lower case.  
After the command is run successfully, the backup directory would contain the Elasticsearch data folder which is stored with the tenant name (for our use case it is _default_) and API Gateway configuration data as a zip file (for our use case it is _backupzipfile.zip_) as seen in the below screenshot. The Elasticsearch data folder is copied from the location we configured in Step 1 by the command.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddava6036dac30a332cb36e452e4fb557fdb.png?api=v2)

### Step 5

1.  After the backup command is executed and the backup is done, stop all the <SOURCE> Elasticsearch nodes. 
2.  Now go to **_<TARGET\_ELASTIC\_SEARCH>\\config_** and configure _path.repo_ property in _elasticsearch.yml_ file for all the <TARGET> Elasticsearch nodes. Start any one of the <TARGET> Elasticsearch nodes.

### Step 6

When the **<TARGET\_ELASTIC\_SEARCH>** is up and running, invoke the below RESP API to create a repository in the <TARGET> machine with tenant name.  
PUT /\_snapshot/tenant\_name  
{ "type": "fs", "settings": { "location": <tenant\_name> } }  
For e.g if we want to create 'a repo for 'default' as tenant name, then the command would be as below.  
PUT   [{+}](http://localhost:9240/_snapshot/default)[http://localhost:9240/\_snapshot/default+](http://localhost:9240/_snapshot/default+)   
{ "type": "fs", "settings": { "location": "default" } }  
After the REST API invocation, a folder with the tenant name (in our use case it is _default_) will be created under the _path.repo_ folder.

### Step 7

This is a preparation step for Elasticsearch data migration. Go to **_backupDestinationDirectory/<tenant\_name>_** directory in Step 3 and copy the contents from Elasticsearch data folder (with tenant name) to the shared repository folder created in Step 5.  
For e.g the below is the Elasticsearch data folder for our use case.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddava100d7da79e8dcf3ce0a72fb8a284246.png?api=v2)

### Step 8

Once the above prerequisites are ready we are now ready to run the migration commands to migrate the Elasticsearch data and the API Gateway configurations. In this step we will migrate the Elasticsearch data.

#### For <TARGET> API Gateway 10.5

Restore the snapshot using the following command in the target instance  
_curl -X POST "localhost:9240/\_snapshot/__<__tenant\_name>/<backupFileName>/\_restore"_  
For e.g if we want to create a repo with 'default' as tenant name, then the command would be as below.  
_POST_ {\_}[http://localhost:9240/\_snapshot/default/backupzipfile/\_restore\_](http://localhost:9240/_snapshot/default/backupzipfile/_restore_)  
As the restore command is asynchronous, you can check its status by using the below command  
\_[http://localhost:9240/\_cluster/health?pretty\_](http://localhost:9240/_cluster/health?pretty_)  
The restore would be completed successfully when the health status becomes yellow from red. Now use this temporary Elasticsearch as source Elasticsearch and migrate the Elasticsearch data by direct mode as mentioned in the section '_Migration using direct mode for standalone_' in any one of the cluster nodes

#### For <TARGET> API Gateway below 10.5

Go to _<TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_ directory in any one of the cluster nodes and run the below command.  
**_$> migrate.bat datastore_**  
After command is run the data store data will be migrated.

### Step 9

After successful migration of Elasticsearch data, run API Gateway configurations migration using the below command.  
**_$> migrate.bat apigateway -srcFile <backupLoc>  fileName.zip  -instanceName <old instance name> -newInstanceName <new instance name>_**  
For e.g **_$> migrate.bat apigateway -srcFile C:\\migration\\backupfolder\\backupzipfile.zip -instanceName default -newInstanceName default_**  
Now the API Gateway configurations are migrated. This should be done for all the API Gateway nodes in the cluster.

### Step 10

This is a post migration step. Follow the below steps.

*   Shutdown the <TARGET> Elasticsearch instances
*   (Optional) If Elasticsearch nodes are configured externally to API Gateway nodes, Start all those externally configured Elasticsearch nodes to setup the cluster
*   Start the <TARGET> API Gateway nodes

Migration configuration
-----------------------

API Gateway customers can modify certain parameters for migration based on their requirements by modifying the property file **_migration.properties_** which is located at <_TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_. This property file is instance specific and customers can have different configurations while migrating different instances.

| **#** | **Property** | **Description** | **Default value** | **Possible values** |
| --- | --- | --- | --- | --- |
| 1   | apigateway.migration.srcTenantName | By default, the source tenant is assumed as default. But If the source API Gateway has multiple tenants, this property can be used to specify the tenant name from which the data has to be migrated to the target tenant. | default | Any available tenant in Elasticsearch |
| 2   | apigateway.migration.batchSize | The batch size with which the data is processed. For e.g if size is 100 then by default 100 documents will be processed first. If the network is slow we can decrease this value and if the network is better we can increase this value. | 100 | Appropriate batch size. It depends on the number of documents and the size of the documents in the data store |
| 3   | apigateway.migration.logLevel | Log level for migration. we can change log level to debug, error etc. | info | info,debug,error,warn,trace |
| 4   | apigateway.migration.reindex.status.check.sleep.interval | Interval configuration in milliseconds. Once the re-indexing process has started from source to target instances, migration process will wake up after every configured sleep interval to check whether the re-indexing is complete. It will check the status of the task id | 5000 | Appropriate sleep interval |

Recovery
--------

During migration, if there is any problem in the execution or any of the handlers got failed, to make sure that assets are migrated properly, we can clean the target instance once and then re run the migration. This clean command will clean the target data store  (the one configured in the config.properties of target machine) . During this procedure all the indices will be removed and this is a non reversible action. Before cleaning the data we will also take a backup of the existing data (you can also restore it). Once cleaned, we can re-run the migration. Also once you trigger the clean command, this process will wait for 5 seconds and if you wrongly triggered it and you want to kill this process you can do that with in that 5 seconds interval.  
![](https://clep.atlassian.net/wiki/download/attachments/557058/worddav7be589f33de8f480f234cd10b2efb15d.png?api=v2)

### Clean command

Before running clean command  
If the <TARGET> is 10.5 and the clean command is executed in a cluster, go to <SOURCE>\\InternalDataStore\\config and configure **_path.repo_** property in elasticsearch.yml file for all the nodes. Make sure that the _path.repo_ is a shared network folder and should be accessible for all the Elasticsearch nodes in the cluster.   
Go to _<TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\bin\\migrate_ and run the below command.  
**_$> migrate.bat clean_**  
Since this command removes all the assets from data store, make sure that the target data store is properly configured in config.properties which is located at <TARGET>\\IntegrationServer\\instances\\default\\packages\\WmAPIGateway\\config\\resources\\elasticsearch.

Troubleshooting
===============

| **#** | **Issue Description** | **Scenario name** | **Reason** | **Remedy** |
| --- | --- | --- | --- | --- |
| 1   | ElasticsearchException\[Error while reindexing APIs. Error type - illegal\_argument\_exception,  <br>reason \[localhost:1240\] not whitelisted in reindex.remote.whitelist\] | Direct - Standalone | Remote re-indexing property missed in target elasticsearch.yml | Add the below property in <TARGET> elasticsearch.yml  <br>reindex.remote.whitelist: <sourcehost>:<sourcehttpport> |
| 2   | Exception thrown during migration operation. Exiting the operation.  <br>No Such command - -srcDir | Direct - Standalone | Wrong command usage | datastore/apigateway argument must be passed  <br>migration.bat apigateway -srcDir C:\\SoftwareAG\_10.1 -instanceName default -newInstanceName default |
| 3   | Deleting the backup folder before migration... Migration Process 0% \[>\] 0/100 (0:00:00) java.io.FileNotFoundException: 15:41:13.580 \[main\] ERROR com.softwareag.apigateway.utility.command.backup.instance.BackupApiGatewayInstance - Error occurred while trying to parse the Manifest file to obtain the version information. java.io.FileNotFoundException:(The system cannot find the file specified) | Backup - Standalone | Existence of isExtract file due to migration failure of previous steps | Delete the isExtract folder in C:\\<<InstallationDir>>\| |
| 4   | elasticsearchclient bean creation exception | All | Make sure the respective Elasticsearch nodes is running |     |
| 5   | \[2018-12-19T12:34:03,892\]\[WARN \]\[o.e.r.VerifyNodeRepositoryAction\]  <br>\[[SAG-2KXGBH2.eur.ad](http://sag-2kxgbh2.eur.ad/).sag1544697848038\] \[default\] failed to verify repository  <br>org.elasticsearch.repositories.RepositoryVerificationException: \[default\] a file written by master to the store \[C:\\SoftwareAG\_10.3\\InternalDataStore\\dasoSnap\\default\] cannot be accessed on the node \[{[SAG-2KXGBH2.eur.ad](http://sag-2kxgbh2.eur.ad/).sag1544697848038} {yL84xqhZQSuFfUmj0GrFbQ}{uGv-BmTBT6e8LCpOsxpGOg} {10.60.37.18}{10.60.37.18:9340}\]. This might indicate that the store \[C:\\SoftwareAG\_10.3\\InternalDataStore\\dasoSnap\\default\] is not shared between this node and the master node or that permissions on the store don't allow reading files written by the master node | Backup Mode - Cluster | Make sure path.repo is a single common location reachable and accessible by all other nodes | Configure single common shared path in elasticsearch.yml (path.repo) |
| 6   | The system cannot find the path specified | All | Make sure that the bat from the proper location | Make sure that you are running the bat from the proper location |
| 7   | 2019-10-16 11:59:36 ERROR ESDataStoreHandler:327 - {"type":"illegal\_argument\_exception","reason":"Remote responded with a chunk that was too large. Use a smaller batch size.","caused\_by":{"type":"content\_too\_long\_exception","reason":"entity content is too long \[185463385\] for the configured buffer limit \[104857600\]"}} | All | The size of the documents selected for reindexing as per the batch size configuration is large | Try with a smaller batch size number (apigateway.migration.batchSize) |
| 8   | Error while getting task details for taks id - 6L6RMNEOQF64lQbNeI7J\_g:683354. Message - task \[6L6RMNEOQF64lQbNeI7J\_g:683354\] isn't running and hasn't stored its results | All | When the space in machine is less than 10% , elasticsearch marks index as readonly hence reindex task fails abruptly | Have sufficient space |

References
==========

*   [{+}](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)[https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html+](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html+)
*   [{+}](https://www.elastic.co/guide/en/elasticsearch/reference/master/docs-reindex.html)[https://www.elastic.co/guide/en/elasticsearch/reference/master/docs-reindex.html+](https://www.elastic.co/guide/en/elasticsearch/reference/master/docs-reindex.html+)

Learn more
==========

*   For backup and restore refer [{+}](https://iwiki.eur.ad.sag/display/RNDWMGDM/Back+up+and+restore+of+API+Gateway+assets)[https://iwiki.eur.ad.sag/display/RNDWMGDM/Back+up+and+restore+of+API+Gateway+assets+](https://iwiki.eur.ad.sag/display/RNDWMGDM/Back+up+and+restore+of+API+Gateway+assets+)