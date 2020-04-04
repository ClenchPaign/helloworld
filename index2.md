RnD API Management : Configuring External Elasticsearch for API Gateway 10.2  

1.  [RnD API Management](index.html)

RnD API Management : Configuring External Elasticsearch for API Gateway 10.2
============================================================================

Created by James, Clench Paign, last modified on Sep 14, 2019

#### Author: Jagadish, Dinesh (External)

#### Supported Versions:

10.2

Overview of the tutorial
========================

This tutorial explains how an Elasticsearch in an external system can be configured in API Gateway as back end data store instead of the readily available EventDataStore that is shipped with API Gateway.

Required knowledge
==================

*   A basic understanding of API Gateway and its communication with EventDataStore for storing data
    
*   A basic understanding of Kibana and its communication with EventDataStore to retrieve analytics data for rendering dashboards in API Gateway
    

Why?
====

API Gateway ships with EventDataStore, a wrapper product over ElasticSeach 5.6.4 (as of API Gateway 10.2), for storing data. But in some cases the API Provider might need API Gateway to connect to an external Elasticsearch for storing data. For example the Provider might already have a licensed Elasticsearch and would like to use that for API Gateway instead of going with the EventDataStore. Another case is, the Provider needs to create a new API Gateway instance in the Integration Server. This would not create a new instance of EventDataStore for it as only one instance of EventDataStore will be installed and exist for all the instances of API Gateway in the same installation. In this case the Provider can install an Elasticsearch instance outside the product or in an external machine and use it for the newly created API Gateway instance.

Prerequisite steps
==================

Complete the below prerequisite steps before configuring external Elasticsearch in API Gateway.

*   Install API Gateway advanced edition of version 10.2 and above
*   Install external Elasticsearch instance of versions above 5.6.x

Details
=======

The below section explains the configuration details need for connecting API Gateway to an external Elastisearch instance.

Changing Kibana configurations to connect to Elasticsearch
----------------------------------------------------------

Now we need to modify the Kibana server configurations to connect to the Elasticsearch for rendering dashboards in API Gateway.

Open _**uiconfiguration.properties**_ file from the directory _**<SAG-Home>\\profiles\\IS\_instance\_name\\apigateway\\config**_. Set _**apigw.kibana.autostart**_ to _**false**_.

![](attachments/530136201/607387741.png)

Open _**kibana.yml**_ file from the directory _**<SAG-Home>\\profiles\\IS\_instance\_name\\apigateway\\dashboard\\config\\kibana.yml**_ . Set elasticsearch.url: http://<external elasticsearch host:port>, for e.g [http://localhost:9200](http://localhost:9200)

![](attachments/530136201/530136238.png)

Now start the Kibana manually by running the _**kibanat.bat**_ in the location _**<SAG-Home>\\profiles\\IS\_instance\_name\\apigateway\\dashboard\\bin.**_

_**![](attachments/530136201/607387742.png)  
**_

Changing API Gateway configurations to connect to Elasticsearch
---------------------------------------------------------------

Follow the below steps to change the API Gateway configurations to connect to EventDataStore.

Go to _**<SAG\_HOME>/IntegrationServer/instances/default/packages/WmAPIGateway/config/resources/elasticsearch**_ and open _**config.properties**_ file.

pg.gateway.elasticsearch.autostart=false  
pg.gateway.elasticsearch.hosts=<external elasticsearch host:port>

![](attachments/530136201/530136228.png)

After making the configurations, start the Elasticsearch. After Elasticsearch is up and running, start the API Gateway. Now you would be able to login to API Gateway, create APIs and see the analytics page for dashboards.

Limitations
===========

*   These configurations are applicable for API Gateway versions 10.2 and above. For 10.1 and below refer [Configuring External Elasticsearch for API Gateway 10.1](https://iwiki.eur.ad.sag/display/RNDWMGDM/Configuring+External+Elasticsearch+for+API+Gateway+10.1)

References
==========

*   Refer [https://www.elastic.co/guide/en/elasticsearch/reference/5.6/zip-windows.html](https://www.elastic.co/guide/en/elasticsearch/reference/5.6/zip-windows.html) for Elasticsearch installation

Learn more
==========

*   Refer [Configuring External Elasticsearch for API Gateway 10.1](https://iwiki.eur.ad.sag/display/RNDWMGDM/Configuring+External+Elasticsearch+for+API+Gateway+10.1) for configuring external Elasticsearch for API Gateway version 10.1 and below
*   Refer [Securing Elasticsearch for API Gateway 10.2](https://iwiki.eur.ad.sag/display/RNDWMGDM/Securing+Elasticsearch+for+API+Gateway+10.2) for a tutorial on securing the Elasticsearch instance

  

Attachments:
------------

![](images/icons/bullet_blue.gif) [image2018-6-29\_10-33-13.png](attachments/530136201/530136226.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-6-29\_10-35-32.png](attachments/530136201/530136527.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-6-29\_10-41-23.png](attachments/530136201/530136526.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-6-29\_10-41-23.png](attachments/530136201/530136238.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-6-29\_10-35-32.png](attachments/530136201/530136228.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-9-14\_13-28-15.png](attachments/530136201/607387741.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2019-9-14\_13-32-48.png](attachments/530136201/607387742.png) (image/png)  

Document generated by Confluence on Apr 03, 2020 08:52

[Atlassian](http://www.atlassian.com/)