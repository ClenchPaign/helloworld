# API Gateway Service Management Service


<a name="overview"></a>
## Overview
API Gateway Service Management Service allows you to manage the APIs in the API Gateway. Any user with the 'Manage APIs' functional privilege can manage the APIs in the API Gateway. By default, the users who are part of either API-Gateway-Providers or API-Gateway-Administrators groups will have this privilege.

API Gateway supports four types of APIs - REST APIs, SOAP APIs, WebSocket APIs and OData APIs. REST APIs can be created by providing the swagger (file/url), openAPI (file/url), raml (file/url) or can be created from scratch. SOAP APIs can be created using the WSDL (file/url). If the API definitions has reference schemas, then an archive containing all the definitions can be provided as an input. WebSocket APIs can be created from scratch. OData APIs can be created using their service document or metadata document url.

This service provides you with the options to create, update, read and delete of all the above API types.

An API can either be in an Active or an InActive state. An Active state indicates that the API is available for consumers. The users can use this service to activate or deactivate the API. Post activation, API Gateway generates 'Gateway Endpoints' which can be used by the API consumers to access the API.  Generally API consumers use their applications to consume the APIs.

This service can also be used to manage the API Scopes. An API Scope is a collection of resources or operations in the API. Users can create multiple scopes for a single API.

Once the API is created, users can enforce the access restrictions and other rules on the API by add the policies to the API. Policies can be attached to REST, SOAP and OData APIs.  Refer to the Policy Management API documentation for more details on the policies. Refer to the Document Management API documentation for more details on attaching documents to an API.

This service can also be used to publish/unpublish the APIs to/from a service registry.  An API in an active state can be registered (published) to one or more service registries.


### Version information
*Version* : 10.5


### URI scheme
*Host* : localhost:5555  
*BasePath* : /rest/apigateway  
*Schemes* : HTTP


### Consumes

* `application/json`


### Produces

* `application/json`




<a name="paths"></a>
## Paths

<a name="createapi"></a>
### POST /apis

#### Description
This REST operation is used to create an API by importing a file, url or from scratch


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**FormData**|**apiDescription**  <br>*optional*|Description of the API|string|
|**FormData**|**apiName**  <br>*required*|Name of the API|string|
|**FormData**|**apiVersion**  <br>*optional*|Version of the API|string|
|**FormData**|**file**  <br>*required*|Input swagger / raml / wsdl file to be imported|file|
|**FormData**|**rootFileName**  <br>*optional*|Name of the main file in the zip. Required only when the input file is zip format|string|
|**FormData**|**type**  <br>*required*|Input file type|enum (swagger, raml, wsdl, openapi)|
|**Body**|**body**  <br>*required*|API request payload|[InputAPI](#inputapi)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the created API object|[APIResponseCreate](#apiresponsecreate)|
|**400**|This status code shows when the user missed the mandatory fields like type, file/url/apiDefinition in the request or provide a invalid request body|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|


#### Consumes

* `application/json`
* `multipart/form-data`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ],
        "teams": [
            {
                "id": "Administrators",
                "name": "Administrators",
                "canDelete": "false"
            },
            {
                "id": "Default",
                "name": "Default",
                "canDelete": "true"
            }
        ]   }
}
```


<a name="getapis"></a>
### GET /apis

#### Description
Get all APIs or subset of APIs


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Query**|**apiIds**  <br>*optional*|API Ids for the API to be retrieved|string|
|**Query**|**from**  <br>*optional*|Starting index from the list of APIs to be retrieved|integer|
|**Query**|**size**  <br>*optional*|Number of APIs to be retrieved|integer|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the list of all APIs|< [APIResponseDelete](#apiresponsedelete) > array|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "apiResponse": [
    {
      "api": {
        "apiName": "ChuckNorrisAPI",
        "apiVersion": "v2",
        "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
        "isActive": false,
        "type": "REST",
        "publishedPortals": [],
        "systemVersion": 2,
        "id": "46df4227-a100-486c-9580-0bf388ec6ec7"
      },
      "responseStatus": "SUCCESS"
    },
    {
      "api": {
        "apiName": "ChuckNorrisAPI",
        "apiVersion": "1.0",
        "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
        "isActive": false,
        "type": "REST",
        "publishedPortals": [],
        "systemVersion": 1,
        "id": "25fb937a-8360-41ab-8be5-987b14fe631d"
      },
      "responseStatus": "SUCCESS",
  "teams": [
                {
                    "id": "Administrators",
                    "name": "Administrators",
                    "canDelete": "false"
                },
                {
                    "id": "Default",
                    "name": "Default",
                    "canDelete": "true"
                }
            ]
      }
  ]
}
```


<a name="deleteapis"></a>
### DELETE /apis

#### Description
Delete the inactive APIs


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Query**|**apiIds**  <br>*required*|API Ids for the APIs to be deleted. Multiple API ids combined by comma|string||
|**Query**|**forceDelete**  <br>*optional*|Flag for force delete. Required when API is associated with some applications|boolean|`"true"`|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the apiId along with the error when unsuccessful|< [APIResponseDelete](#apiresponsedelete) > array|
|**204**|Success|No Content|
|**400**|This response code returns when the mandatory parameter apiIds is missing in the query parameter|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="getapi"></a>
### GET /apis/{apiId}

#### Description
Retrieve an API based on the API id.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be retrieved|string|
|**Query**|**format**  <br>*optional*|Output format of the API. If the value is 'swagger', you get a API definition in swagger format. If the value is 'raml', you get a raml document. If the value is 'openapi', you get a open API document. If the value is 'odata', you get a zip file holding the OData metadata and service document.|string|
|**Query**|**url**  <br>*optional*|User selected endpoint for API definition in swagger/raml format.|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|If the format is swagger, returns the swagger content in json and raml returns the raml content in yaml. If the format is openapi, returns the open api content in json. If the format is odata, you get a zip file holding the OData metadata and service document.|[APIResponseGetAPI](#apiresponsegetapi)|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
       
       }
        ],
        "teams": [
            {
                "id": "Administrators",
                "name": "Administrators",
                "canDelete": "false"
            },
            {
                "id": "Default",
                "name": "Default",
                "canDelete": "true"
            }
        ]    }
}
```


<a name="updateapi"></a>
### PUT /apis/{apiId}

#### Description
This REST operation is used to update an API by importing a file, url or inline.

While updating the API, visibility of the operations can be set by enabling or disabling the operations. Disabled operations will not be exposed to the customers. By default, all the operations are exposed to the consumers.

When updating the API using file or url, API Gateway overwrite the resources/operations for the API. But it will retain the maturity state, scopes, visibility and if API mocking is enabled, then default mocked responses, mocked conditions and IS services will also be retained.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be updated|string|
|**FormData**|**apiDescription**  <br>*optional*|Description of the API|string|
|**FormData**|**apiName**  <br>*required*|Name of the API|string|
|**FormData**|**apiVersion**  <br>*optional*|Version of the API|string|
|**FormData**|**file**  <br>*required*|Input swagger / raml / wsdl file|file|
|**FormData**|**rootFileName**  <br>*optional*|Name of the main file in the zip. Required when the input file is zip format|string|
|**FormData**|**type**  <br>*required*|Input file type|enum (swagger, raml, wsdl, openapi)|
|**Body**|**body**  <br>*required*|API request payload|[GatewayAPI](#gatewayapi)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the updated API object|[APIResponseCreate](#apiresponsecreate)|
|**400**|This status code shows when the user missed the mandatory fields like type, file/url/apiDefinition in the request or provide a invalid request body|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Consumes

* `application/json`
* `multipart/form-data`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ]
    }
}
```


<a name="deleteapi"></a>
### DELETE /apis/{apiId}

#### Description
Delete the inactive API


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be deleted|string||
|**Query**|**forceDelete**  <br>*optional*|Flag for force delete. Required when API is associated with some applications|boolean|`"true"`|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the apiId along with the error when unsuccessful|[APIResponseDelete](#apiresponsedelete)|
|**204**|Success|No Content|
|**400**|This response code returns when the deleted API is published to API portal or in active state|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="activateapi"></a>
### PUT /apis/{apiId}/activate

#### Description
Activate an API so that API is exposed to consumers


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be activated|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successful activation|[APIResponse](#apiresponse)|
|**400**|This status code shows when the API is already in activated state or when no operations/resources are present or none are enabled|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ]
    }
}
```


<a name="getapplications"></a>
### GET /apis/{apiId}/applications

#### Description
Retrieves the list of registered applications of an API


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to find the associated applications|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the list of associated applications|< [Application](#application) > array|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "applications": [
    {
      "name": "app1",
      "description": null,
      "contactEmails": [],
      "identifiers": [],
      "siteURLs": [],
      "version": "1.0",
      "id": "ae48cd69-421e-4bdf-a4d0-e86996a78f68",
      "created": "2017-03-13 13:12:03 GMT",
      "lastupdated": null,
      "consumingAPIs": [
        "25fb937a-8360-41ab-8be5-987b14fe631d"
      ],
      "accessTokens": {
        "apiAccessKey_credentials": {
          "apiAccessKey": "cec4b46b-3569-4f73-a561-172dd67c182a",
          "expirationInterval": null
        },
        "oauth_credentials": {
          "clientID": "40b78ed3-d171-4bd3-99db-51dd2fa71753",
          "clientSecret": "024b9525-6526-45c8-a66c-d192442064e1",
          "clientName": "app1-6b753c2a-0567-462d-a4ea-1b143ab7a381",
          "scopes": [
            "25fb937a-8360-41ab-8be5-987b14fe631d"
          ],
          "token_lifetime": "3600",
          "token_refresh_limit": "0",
          "redirect_uris": [
            "https://placeholder_redirect_uri"
          ],
          "Type": "confidential"
        }
      }
    }
  ]
}
```


<a name="deactivateapi"></a>
### PUT /apis/{apiId}/deactivate

#### Description
Deactivate an API so that API is not exposed to consumers


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be deactivated|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successful deactivation|[APIResponse](#apiresponse)|
|**400**|This status code shows when the API is already in de-activated state|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ]
    }
}
```


<a name="getassociatedglobalpolicies"></a>
### GET /apis/{apiId}/globalPolicies

#### Description
Retrieves the list of active global policies applicable to this API


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to find the list of applicable global policies|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the list of global policy names|[APIResponseGetGlobalPolicies](#apiresponsegetglobalpolicies)|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "globalPolicies": [
    "GlobalLogInvocationPolicy"
  ]
}
```


<a name="notifyapiimplementation"></a>
### PUT /apis/{apiId}/implementation

#### Description
An API Provider tool can use this operation to update the API in APIGateway after completion of its implementation in their end


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be updated|string|
|**Query**|**maturityState**  <br>*optional*|Value of the 'maturity state' attribute of the API. The 'maturity state' of the API can be set to its one of possible value (from its defines values in extended settings configuration) to depict the completion its implementation|string|
|**Query**|**nativeBaseURL**  <br>*optional*|Base URL of the native API|string|
|**Query**|**overwriteAlias**  <br>*optional*|Flag to replace the endpoint alias that is used in the routing policy of the API with the given value of 'nativeBaseURL' parameter.|boolean|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the updated API object|[APIResponse](#apiresponse)|
|**400**|This status code shows when the user missed the mandatory fields like type, file/url/apiDefinition in the request or provide a invalid request body|No Content|
|**401**||No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Consumes

* `multipart/form-data`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="disablemockapi"></a>
### PUT /apis/{apiId}/mock/disable

#### Description
Once API is disabled from mocking capability, at runtime all the API invocations are redirected to the native service instead of sending the mocked response


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be deactivated|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successful disabling mocking of an API|[APIResponse](#apiresponse)|
|**400**|This status code shows when the API is already in activated state or in mocked state|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "API to demonstrate mocking functionality in international developers day",
                    "version": "v1",
                    "title": "API_MOCKING"
                },
                "host": "localhost",
                "schemes": [
                    "http"
                ],
                "consumes": [
                    "application/json"
                ],
                "security": [],
                "paths": {
                    "/conditionBasedMockedResponse": {
                        "post": {
                            "summary": "Configure condition and mocked response",
                            "operationId": "conditionBasedMockedResponse",
                            "produces": [
                                "text/plain"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response",
                                    "content": {
                                        "text/plain": {
                                            "example": "No condition evaluates to true. \nSo API-Gateway sent this default response."
                                        }
                                    }
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "text/plain": "No condition evaluates to true. \nSo API-Gateway sent this default response."
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/conditionBasedMockedResponse",
                        "enabled": true
                    },
                    "/customESBMockedResponse": {
                        "post": {
                            "summary": "Configure custom ESB mocked response",
                            "operationId": "customESBMockedResponse",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response"
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": ""
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/customESBMockedResponse",
                        "enabled": true
                    },
                    "/dynamicMockedResponse": {
                        "post": {
                            "summary": "Dynamic mocked response set",
                            "operationId": "dynamicMockedResponse",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response"
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": ""
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/dynamicMockedResponse",
                        "enabled": true
                    },
                    "/staticMockedResponse": {
                        "post": {
                            "summary": "Default mocked response from example",
                            "operationId": "generateFromExample",
                            "produces": [
                                "application/json",
                                "application/xml"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response generated from example",
                                    "content": {
                                        "application/json": {
                                            "example": "{\"resource\" : \"/generateFromExample\",\"description\" : \"Default mocked response from example for status code 200\"}"
                                        },
                                        "application/xml": {
                                            "example": "<root><resource>/generateFromExample</resource><description>Default mocked response from example for status code 200</description></root>"
                                        }
                                    }
                                },
                                "201": {
                                    "description": "201 response generated from schema",
                                    "content": {
                                        "application/json": {
                                            "schema": {
                                                "$ref": "#/components/schemas/Pet"
                                            }
                                        },
                                        "application/xml": {
                                            "schema": {
                                                "$ref": "#/components/schemas/Pet"
                                            }
                                        }
                                    }
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": "{\"resource\" : \"/generateFromExample\",\"description\" : \"Default mocked response from example for status code 200\"}",
                                        "application/xml": "<root><resource>/generateFromExample</resource><description>Default mocked response from example for status code 200</description></root>"
                                    }
                                },
                                "201": {
                                    "responseBody": {
                                        "application/json": "{\"birthday\":2059397944,\"name\":\"\"}",
                                        "application/xml": "<birthday>921604684</birthday><name/>"
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/staticMockedResponse",
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "http://localhost"
                    }
                ],
                "components": {
                    "schemas": {
                        "Pet": {
                            "required": [
                                "name"
                            ],
                            "type": "object",
                            "properties": {
                                "birthday": {
                                    "type": "integer",
                                    "format": "int32"
                                },
                                "name": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "http://localhost",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "APIMocking",
            "apiVersion": "v1",
            "apiDescription": "API to demonstrate mocking functionality in international developers day",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "19773e29-2838-4efc-aa04-793b48f4d22b"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-11-01 13:44:58 GMT",
            "systemVersion": 1,
            "mockService": {
                "enableMock": false
            },
            "id": "afd8eb5e-bba8-447b-8e28-76aac23ba074"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "v1",
                "apiId": "afd8eb5e-bba8-447b-8e28-76aac23ba074"
            }
        ]
    }
}
```


<a name="enablemockapi"></a>
### PUT /apis/{apiId}/mock/enable

#### Description
In API Gateway, you can mock an API implementation. API Gateway lets you mock an API by simulating the native service. API Mocking is useful feature in API first approach, where in the provider may choose to expose the mocked API to the consumers when the actual API doesn't exist or isn't complete. 
 In API Gateway, when you enable mocking for an API, a default mock response is created for each combination of resource, operation, status code and content-type based on the example and schema set in the API definition. As an API Provider, you can add or modify the default mock responses.

You can specify conditions at the operation level and configure IS services at the API level for a mocked API in the update API operation. At runtime, when the mocked API is invoked instead of calling the native service, API Gateway returns the mocked response to the consumer based on the below priorities:
1. If any of the conditions for the invoked operation satisfies, API Gateway returns the associated mocked response.
2. If no condition is specified or none of the condition for the invoked operation is satisfied, then API Gateway will return 
a. the response from an IS service, if an IS service is configured b. default mocked response, if no IS services are configured


#### Parameters

|Type|Name|Description|Schema|Default|
|---|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be activated|string||
|**Query**|**retainDefaultMockResponses**  <br>*optional*|Flag to retain generated mocked responses. When this is set to true, default mocked responses will be retained. If it's set to false, new default mocked responses will be generated using the examples, schema in the API|boolean|`"false"`|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successfully enabling mocking of an API|[APIResponse](#apiresponse)|
|**400**|This status code shows when the API is already in activated state or when invalid json or xml is provided in the example part of the operation|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "API to demonstrate mocking functionality in international developers day",
                    "version": "v1",
                    "title": "API_MOCKING"
                },
                "host": "localhost",
                "schemes": [
                    "http"
                ],
                "consumes": [
                    "application/json"
                ],
                "security": [],
                "paths": {
                    "/conditionBasedMockedResponse": {
                        "post": {
                            "summary": "Configure condition and mocked response",
                            "operationId": "conditionBasedMockedResponse",
                            "produces": [
                                "text/plain"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response",
                                    "content": {
                                        "text/plain": {
                                            "example": "No condition evaluates to true. \nSo API-Gateway sent this default response."
                                        }
                                    }
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "text/plain": "No condition evaluates to true. \nSo API-Gateway sent this default response."
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/conditionBasedMockedResponse",
                        "enabled": true
                    },
                    "/customESBMockedResponse": {
                        "post": {
                            "summary": "Configure custom ESB mocked response",
                            "operationId": "customESBMockedResponse",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response"
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": ""
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/customESBMockedResponse",
                        "enabled": true
                    },
                    "/dynamicMockedResponse": {
                        "post": {
                            "summary": "Dynamic mocked response set",
                            "operationId": "dynamicMockedResponse",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response"
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": ""
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/dynamicMockedResponse",
                        "enabled": true
                    },
                    "/staticMockedResponse": {
                        "post": {
                            "summary": "Default mocked response from example",
                            "operationId": "generateFromExample",
                            "produces": [
                                "application/json",
                                "application/xml"
                            ],
                            "responses": {
                                "200": {
                                    "description": "200 response generated from example",
                                    "content": {
                                        "application/json": {
                                            "example": "{\"resource\" : \"/generateFromExample\",\"description\" : \"Default mocked response from example for status code 200\"}"
                                        },
                                        "application/xml": {
                                            "example": "<root><resource>/generateFromExample</resource><description>Default mocked response from example for status code 200</description></root>"
                                        }
                                    }
                                },
                                "201": {
                                    "description": "201 response generated from schema",
                                    "content": {
                                        "application/json": {
                                            "schema": {
                                                "$ref": "#/components/schemas/Pet"
                                            }
                                        },
                                        "application/xml": {
                                            "schema": {
                                                "$ref": "#/components/schemas/Pet"
                                            }
                                        }
                                    }
                                }
                            },
                            "mockedResponses": {
                                "200": {
                                    "responseBody": {
                                        "application/json": "{\"resource\" : \"/generateFromExample\",\"description\" : \"Default mocked response from example for status code 200\"}",
                                        "application/xml": "<root><resource>/generateFromExample</resource><description>Default mocked response from example for status code 200</description></root>"
                                    }
                                },
                                "201": {
                                    "responseBody": {
                                        "application/json": "{\"birthday\":2059397944,\"name\":\"\"}",
                                        "application/xml": "<birthday>921604684</birthday><name/>"
                                    }
                                }
                            },
                            "enabled": true
                        },
                        "parameters": [],
                        "displayName": "/staticMockedResponse",
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "http://localhost"
                    }
                ],
                "components": {
                    "schemas": {
                        "Pet": {
                            "required": [
                                "name"
                            ],
                            "type": "object",
                            "properties": {
                                "birthday": {
                                    "type": "integer",
                                    "format": "int32"
                                },
                                "name": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "http://localhost",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "APIMocking",
            "apiVersion": "v1",
            "apiDescription": "API to demonstrate mocking functionality in international developers day",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "19773e29-2838-4efc-aa04-793b48f4d22b"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-11-01 13:44:58 GMT",
            "systemVersion": 1,
            "mockService": {
                "enableMock": true
            },
            "id": "afd8eb5e-bba8-447b-8e28-76aac23ba074"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "v1",
                "apiId": "afd8eb5e-bba8-447b-8e28-76aac23ba074"
            }
        ]
    }
}
```


<a name="downloadproviderspecification"></a>
### GET /apis/{apiId}/providerspecification

#### Description
Downloads the provider specification of REST and SOAP based APIs. Provider specification is nothing but, the specification file (in swagger or wsdl format) with out the concrete API Gateway endpoint and contains all resources/methods/operation irrespective of whether they are exposed to consumer


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to retrieve the versions|string|
|**Query**|**format**  <br>*required*|Output format of the API specification. For REST APIs the value is 'swagger'; for SOAP APIs use the value as 'wsdl'|enum (swagger, wsdl)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|If the format is swagger, returns the swagger content in json. If the format is wsdl, returns the wsdl content in xml.|[APIResponseGetAPI](#apiresponsegetapi)|
|**401**||No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="publishapi"></a>
### PUT /apis/{apiId}/publish

#### Description
This REST operation is used to publish API to the registered API Portal


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be published|string|
|**Body**|**body**  <br>*required*|API publish request payload|[InputPublish](#inputpublish)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successful publish|[APIResponseCreate](#apiresponsecreate)|
|**400**|This status code shows when the user missed the mandatory portalGatewayId or invalid portalGatewayId in the request body|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Consumes

* `application/json`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "apiResponse": {
    "api": {
      "apiDefinition": {
        "type": "rest",
        "info": {
          "vendorExtensions": {},
          "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
          "version": "1.0",
          "title": "ChuckNorrisAPI"
        },
        "host": "api.chucknorris.io",
        "basePath": "/jokes",
        "schemes": [
          "https"
        ],
        "paths": {
          "/random": {
            "get": {
              "summary": "GET",
              "description": "",
              "operationId": "GET",
              "produces": [
                "application/json"
              ],
              "parameters": [],
              "responses": {},
              "enabled": true
            },
            "enabled": true
          }
        },
        "definitions": {}
      },
      "nativeEndpoint": [
        {
          "passSecurityHeaders": true,
          "uri": "https://api.chucknorris.io/jokes",
          "connectionTimeoutDuration": 0,
          "alias": false
        }
      ],
      "apiName": "ChuckNorrisAPI",
      "apiVersion": "1.0",
      "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
      "maturityState": "Beta",
      "isActive": false,
      "type": "REST",
      "owner": "Administrator",
      "policies": [
        "879068cd-8628-4f2a-b903-4e6613ca12ba"
      ],
      "referencedFiles": {
        "ChuckNorrisAPI.json": "{\r\n  \"swagger\": \"2.0\",\r\n  \"info\": {\r\n    \"description\": \"Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.\",\r\n    \"title\": \"ChuckNorrisAPI\",\r\n    \"version\": \"1.0\"\r\n  },\r\n  \"host\": \"api.chucknorris.io\",\r\n  \"basePath\": \"/jokes\",\r\n  \"schemes\": [\r\n    \"https\"\r\n  ],\r\n  \"paths\": {\r\n    \"/random\": {\r\n      \"get\": {\r\n        \"summary\": \"GET\",\r\n        \"deprecated\": false,\r\n        \"produces\": [\r\n          \"application/json\"\r\n        ],\r\n        \"description\": \"\",\r\n        \"operationId\": \"GET\"\r\n      }\r\n    }\r\n  }\r\n}\r\n"
      },
      "scopes": [],
      "publishedPortals": [],
      "creationDate": "2017-03-13 09:38:30 GMT",
      "systemVersion": 1,
      "id": "25fb937a-8360-41ab-8be5-987b14fe631d",
      "oauth2ScopeName": "25fb937a-8360-41ab-8be5-987b14fe631d"
    },
    "responseStatus": "SUCCESS"
  }
}
```


<a name="getscopes"></a>
### GET /apis/{apiId}/scopes

#### Description
An API Scope is a collection of resources or operations in an API. Users can create multiple scopes for a single API. Policies can be attached to an API level or scope level. This method retrieves the scopes of an API.

You can create, modify or delete the scopes in the update API operation using PUT /api/{apiId}


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to retrieve the versions|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns API scopes|< [ScopeResourceIndex](#scoperesourceindex) > array|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "scopeReferences": [
    {
      "references": [
        {
          "resourcePath": "/random",
          "supportedOperations": []
        }
      ],
      "scope": {
        "name": "Get_Scopes",
        "description": "Dummy description of the scope",
        "policies": [
          "db1a42f4-e038-4a1b-82f4-8fee6fbd5687"
        ]
      }
    }
  ]
}
```


<a name="getscopebyscopename"></a>
### GET /apis/{apiId}/scopes/{scopeName}

#### Description
Retrieve scopes of an API based on the scope name


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to retrieve the versions|string|
|**Path**|**scopeName**  <br>*required*|Name of the scope|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns API scopes|< [ScopeResourceIndex](#scoperesourceindex) > array|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway or scopeName is not found in the list of scopes|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "scopeReferences": [
    {
      "references": [
        {
          "resourcePath": "/random",
          "supportedOperations": []
        }
      ],
      "scope": {
        "name": "Get_Scopes",
        "description": "Dummy description of the scope",
        "policies": [
          "db1a42f4-e038-4a1b-82f4-8fee6fbd5687"
        ]
      }
    }
  ]
}
```


<a name="getsource"></a>
### GET /apis/{apiId}/source

#### Description
Download the API definition that was used to create the API. This is applicable only for SOAP APIs.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to download the source content|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the source files along with the root file name|< [Multipart](#multipart) > array|
|**400**|This status code returns when the specified API is not a SOAP API|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `multipart/mixed`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "multipart/mixed" : "Message-ID: <296841806.5.1489555643275.JavaMail.MRIZ@MCMRIZ01>\r\nMIME-Version: 1.0\r\nContent-Type: multipart/mixed; \r\n\tboundary=\"----=_Part_4_1098332532.1489555643274\"\r\n\r\n------=_Part_4_1098332532.1489555643274\r\ncontent-type: application/zip\r\nContent-Disposition: attachment; filename=\"echoService.zip\"\r\n\r\nfile content in zip format\r\n------=_Part_4_1098332532.1489555643274\r\ncontent-type: text/plain\r\nContent-Disposition: inline; name=\"rootFileName\"\r\n\r\necho.wsdl\r\n------=_Part_4_1098332532.1489555643274--"
}
```


<a name="unpublishapi"></a>
### PUT /apis/{apiId}/unpublish

#### Description
Unpublish API from the registered API Portal


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be unpublished|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API object after successful unpublish|[APIResponseCreate](#apiresponsecreate)|
|**400**|This status code shows when the user missed the mandatory portalGatewayId or invalid portalGatewayId in the request body|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "apiResponse": {
    "api": {
      "apiDefinition": {
        "type": "rest",
        "info": {
          "vendorExtensions": {},
          "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
          "version": "1.0",
          "title": "ChuckNorrisAPI"
        },
        "host": "api.chucknorris.io",
        "basePath": "/jokes",
        "schemes": [
          "https"
        ],
        "paths": {
          "/random": {
            "get": {
              "summary": "GET",
              "description": "",
              "operationId": "GET",
              "produces": [
                "application/json"
              ],
              "parameters": [],
              "responses": {},
              "enabled": true
            },
            "enabled": true
          }
        },
        "definitions": {}
      },
      "nativeEndpoint": [
        {
          "passSecurityHeaders": true,
          "uri": "https://api.chucknorris.io/jokes",
          "connectionTimeoutDuration": 0,
          "alias": false
        }
      ],
      "apiName": "ChuckNorrisAPI",
      "apiVersion": "1.0",
      "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
      "maturityState": "Beta",
      "isActive": false,
      "type": "REST",
      "owner": "Administrator",
      "policies": [
        "879068cd-8628-4f2a-b903-4e6613ca12ba"
      ],
      "referencedFiles": {
        "ChuckNorrisAPI.json": "{\r\n  \"swagger\": \"2.0\",\r\n  \"info\": {\r\n    \"description\": \"Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.\",\r\n    \"title\": \"ChuckNorrisAPI\",\r\n    \"version\": \"1.0\"\r\n  },\r\n  \"host\": \"api.chucknorris.io\",\r\n  \"basePath\": \"/jokes\",\r\n  \"schemes\": [\r\n    \"https\"\r\n  ],\r\n  \"paths\": {\r\n    \"/random\": {\r\n      \"get\": {\r\n        \"summary\": \"GET\",\r\n        \"deprecated\": false,\r\n        \"produces\": [\r\n          \"application/json\"\r\n        ],\r\n        \"description\": \"\",\r\n        \"operationId\": \"GET\"\r\n      }\r\n    }\r\n  }\r\n}\r\n"
      },
      "scopes": [],
      "publishedPortals": [],
      "creationDate": "2017-03-13 09:38:30 GMT",
      "systemVersion": 1,
      "id": "25fb937a-8360-41ab-8be5-987b14fe631d",
      "oauth2ScopeName": "25fb937a-8360-41ab-8be5-987b14fe631d"
    },
    "responseStatus": "SUCCESS"
  }
}
```


<a name="createversion"></a>
### POST /apis/{apiId}/versions

#### Description
Create a new version of an API and retain applications if required


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to be versioned|string|
|**Body**|**body**  <br>*required*|Create version request payload|[InputVersion](#inputversion)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Return the newly created version of the API|[APIResponse](#apiresponse)|
|**400**|This status code returns when the specified api is not the latest version or if the newApiVersion is empty|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|Not Found|No Content|


#### Consumes

* `application/json`


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ]
    }
}
```


<a name="getversions"></a>
### GET /apis/{apiId}/versions

#### Description
Retrieve all the versions of the API


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Path**|**apiId**  <br>*required*|API Id for the API to retrieve the versions|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the API versions|[APIResponseDelete](#apiresponsedelete)|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that the apiId specified is not found in the API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
    "apiResponse": {
        "api": {
            "apiDefinition": {
                "type": "rest",
                "info": {
                    "description": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
                    "version": "1.0",
                    "title": "ChuckNorrisAPI"
                },
                "host": "api.chucknorris.io",
                "basePath": "/jokes",
                "schemes": [
                    "https"
                ],
                "security": [],
                "paths": {
                    "/random": {
                        "get": {
                            "summary": "GET",
                            "description": "",
                            "operationId": "GET",
                            "produces": [
                                "application/json"
                            ],
                            "responses": {},
                            "enabled": true
                        },
                        "enabled": true
                    }
                },
                "securityDefinitions": {},
                "definitions": {},
                "baseUriParameters": [],
                "externalDocs": [],
                "servers": [
                    {
                        "url": "https://api.chucknorris.io/jokes"
                    }
                ],
                "components": {
                    "schemas": {}
                }
            },
            "nativeEndpoint": [
                {
                    "passSecurityHeaders": true,
                    "uri": "https://api.chucknorris.io/jokes",
                    "connectionTimeoutDuration": 0,
                    "alias": false
                }
            ],
            "apiName": "ChuckNorris",
            "apiVersion": "1.0",
            "apiDescription": "Chuck Norris facts are satirical factoids about martial artist and actor Chuck Norris that have become an Internet phenomenon and as a result have become widespread in popular culture. The 'facts' are normally absurd hyperbolic claims about Norris' toughness, attitude, virility, sophistication, and masculinity.",
            "maturityState": "Beta",
            "isActive": false,
            "type": "REST",
            "owner": "Administrator",
            "policies": [
                "08afbfa9-78e1-4c23-bb19-c0012464047e"
            ],
            "scopes": [],
            "publishedPortals": [],
            "creationDate": "2018-09-03 11:56:21 GMT",
            "systemVersion": 1,
            "id": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
        },
        "responseStatus": "SUCCESS",
        "versions": [
            {
                "versionNumber": "1.0",
                "apiId": "badc18e6-446f-4aa3-96cd-33e46bd40fb5"
            }
        ]
    }
}
```


<a name="getintegrationserverpublishinfo"></a>
### GET /integrationServer/publish

#### Description
Retrieve the integration server publish information for the API. Only REST and SOAP APIs are supported.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Query**|**apiId**  <br>*required*|API Id of the API for which IntegrationServerPublishInfo is to be fetched|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the integration server publish info for the API|[ServiceRegistryPublishGetResponse](#serviceregistrypublishgetresponse)|
|**401**||No Content|
|**404**|This status code indicates that Publish Info for the apiId specified is not found in API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="publishtointegrationserver"></a>
### PUT /integrationServer/publish

#### Description
Publish one or more APIs to one or more integration servers. Only REST and SOAP APIs are supported.


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Body**|**body**  <br>*required*|Integration server publish payload|[InputIntegrationServerPublish](#inputintegrationserverpublish)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the status of the publish operations given in the request.|[ServiceRegistryPublishPutResponse](#serviceregistrypublishputresponse)|
|**400**|This status code indicates an invalid request body|No Content|
|**401**||No Content|
|**404**|This status code indicates that API with given apiId is not found in API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


<a name="getserviceregistrypublishinfo"></a>
### GET /serviceRegistry/publish

#### Description
Retrieve the service registry publish information for the API


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Query**|**apiId**  <br>*required*|API Id of the API for which ServiceRegistryPublishInfo is to be fetched|string|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the service registry publish info for the API|[ServiceRegistryPublishGetResponse](#serviceregistrypublishgetresponse)|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that Publish Info for the apiId specified is not found in API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "publishInfo": {
    "apiId": "afe8b72e-e1a5-47c6-9b43-e7f12858c091",
    "serviceRegistryPublishInfo": [
      {
        "serviceRegistryId": "aec973cd-1e4c-4a93-93a4-950e32d39156",
        "status": "PUBLISHED",
        "name": "MyServiceConsul",
        "gatewayEndpoints": [
          {
            "gatewayEndpoint": "http://localhost:5555/ws/calc/1",
            "status": "PUBLISHED"
          },
          {
            "gatewayEndpoint": "http://localhost:1111/ws/calc/1",
            "status": "NEW"
          }
        ]
      }
    ]
  }
}
```


<a name="publishtoserviceregistry"></a>
### PUT /serviceRegistry/publish

#### Description
Publish one or more APIs to one or more service registries


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Body**|**body**  <br>*required*|Service registry publish payload|[InputServiceRegistryPublish](#inputserviceregistrypublish)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the status of the publish operations given in the request.|[ServiceRegistryPublishPutResponse](#serviceregistrypublishputresponse)|
|**400**|This status code indicates an invalid request body|No Content|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|
|**404**|This status code indicates that API with given apiId is not found in API Gateway|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "publishResponse": {
    "apiId": "afe8b72e-e1a5-47c6-9b43-e7f12858c091",
    "apiName": "CalcService",
    "apiVersion": "10.3",
    "serviceRegistryPublishResponses": [
      {
        "serviceRegistryId": "aec973cd-1e4c-4a93-93a4-950e32d39156",
        "serviceRegistryName": "MyServiceConsul",
        "status": "PUBLISHED",
        "gatewayEndpoints": [
          {
            "gatewayEndpoint": "http://localhost:5555/ws/calc/1",
            "status": "PUBLISHED",
          }
        ],
        "success": true,
        "description": "Publish successful"
      }
    ]
  }
}
```


<a name="unpublishfromserviceregistry"></a>
### PUT /serviceRegistry/unpublish

#### Description
Unpublish one or more APIs from one or more service registries


#### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Body**|**body**  <br>*required*|Service registry unpublish payload|[InputServiceRegistryUnpublish](#inputserviceregistryunpublish)|


#### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Returns the service registry unpublish response|[ServiceRegistryUnpublishPutResponse](#serviceregistryunpublishputresponse)|
|**401**|This status code indicates that either user didn't provide right credentials or user doesn't have required privileges to access this API.|No Content|


#### Produces

* `application/json`


#### Security

|Type|Name|
|---|---|
|**basic**|**[Basic](#basic)**|


#### Example HTTP response

##### Response 200
```json
{
  "unpublishResponse": {
    "apiId": "afe8b72e-e1a5-47c6-9b43-e7f12858c091",
    "apiName": "CalcService",
    "apiVersion": "10.3",
    "serviceRegistryUnpublishResponses": [
      {
        "serviceRegistryId": "aec973cd-1e4c-4a93-93a4-950e32d39156",
        "serviceRegistryName": "MyServiceConsul",
        "success": true,
        "description": " Unpublish successful"
      }
    ]
  }
}
```




<a name="definitions"></a>
## Definitions

<a name="api"></a>
### API

|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**serviceRegistryDisplayName**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**uri**  <br>*optional*|< string > array|
|**version**  <br>*optional*|string|


<a name="apiaccesskey"></a>
### APIAccessKey

|Name|Description|Schema|
|---|---|---|
|**apiAccessKey**  <br>*optional*|API access key|string|
|**expirationDate**  <br>*optional*|expiration date of the api key|string|
|**expirationInterval**  <br>*optional*|expiration interval of the api key|string|


<a name="apiresponse"></a>
### APIResponse

|Name|Schema|
|---|---|
|**api**  <br>*optional*|[GatewayAPI](#gatewayapi)|
|**apiId**  <br>*optional*|string|
|**errorReason**  <br>*optional*|string|
|**gatewayEndPoints**  <br>*optional*|< string > array|
|**microgatewayEndPoints**  <br>*optional*|< string > array|
|**portalGatewayDataEntries**  <br>*optional*|object|
|**pubSOAPFlavor**  <br>*optional*|string|
|**reason**  <br>*optional*|object|
|**responseStatus**  <br>*optional*|enum (SUCCESS, ERROR, NOT_FOUND, BAD_REQUEST, PARTIAL_SUCCESS)|
|**restrictViewAsset**  <br>*optional*|boolean|
|**rootFileLocation**  <br>*optional*|string|
|**teams**  <br>*optional*|< [Team](#team) > array|


<a name="apiresponsecreate"></a>
### APIResponseCreate

|Name|Schema|
|---|---|
|**api**  <br>*optional*|[GatewayAPI](#gatewayapi)|
|**apiId**  <br>*optional*|string|
|**errorReason**  <br>*optional*|string|
|**responseStatus**  <br>*optional*|enum (SUCCESS, ERROR, NOT_FOUND, BAD_REQUEST, PARTIAL_SUCCESS)|


<a name="apiresponsedelete"></a>
### APIResponseDelete
This model contains the basics details of an API.


|Name|Description|Schema|
|---|---|---|
|**active**  <br>*optional*||boolean|
|**apiId**  <br>*optional*||string|
|**apiName**  <br>*optional*|API Name|string|
|**apiVersion**  <br>*optional*|API Version|string|
|**errorReason**  <br>*optional*||string|
|**id**  <br>*optional*|API Id|string|
|**publishedPortals**  <br>*optional*|Published portals of an API|< string > array|
|**responseStatus**  <br>*optional*||enum (SUCCESS, ERROR, NOT_FOUND, BAD_REQUEST, PARTIAL_SUCCESS)|
|**systemVersion**  <br>*optional*|System version of an API|integer (int32)|
|**teams**  <br>*optional*|Contains teams belonging to an API.|< [Team](#team) > array|
|**type**  <br>*optional*|API Type|string|


<a name="apiresponsegetapi"></a>
### APIResponseGetAPI

|Name|Schema|
|---|---|
|**api**  <br>*optional*|[GatewayAPI](#gatewayapi)|
|**apiId**  <br>*optional*|string|
|**errorReason**  <br>*optional*|string|
|**gatewayEndPoints**  <br>*optional*|< string > array|
|**responseStatus**  <br>*optional*|enum (SUCCESS, ERROR, NOT_FOUND, BAD_REQUEST, PARTIAL_SUCCESS)|
|**versions**  <br>*optional*|< [Version](#version) > array|


<a name="apiresponsegetglobalpolicies"></a>
### APIResponseGetGlobalPolicies

|Name|Schema|
|---|---|
|**globalPolicies**  <br>*optional*|< string > array|


<a name="abstractparameter"></a>
### AbstractParameter

|Name|Description|Schema|
|---|---|---|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="actionimport"></a>
### ActionImport

|Name|Schema|
|---|---|
|**action**  <br>*optional*|string|
|**enabled**  <br>*optional*|boolean|


<a name="application"></a>
### Application

|Name|Description|Schema|
|---|---|---|
|**accessTokens**  <br>*optional*||[ApplicationToken](#applicationtoken)|
|**applicationID**  <br>*optional*|unique identifier of an application|string|
|**authStrategyIds**  <br>*optional*||< string > array|
|**contactEmails**  <br>*optional*|list of email contacts|< string > array|
|**creationDate**  <br>*optional*|application creation time|string|
|**description**  <br>*optional*|description of the application|string|
|**iconbyteArray**  <br>*optional*|application icon byte array|string|
|**identifiers**  <br>*optional*|list of all application identifiers|< [ApplicationIdentifier](#applicationidentifier) > array|
|**isSuspended**  <br>*optional*|holds the suspended state of an application|boolean|
|**jsOrigins**  <br>*optional*|list of all javascript origins|< string > array|
|**lastModified**  <br>*optional*|last modified time of the application|string|
|**lastUpdated**  <br>*optional*|last modified time of the application in milliseconds|integer (int64)|
|**name**  <br>*optional*|name of the application|string|
|**owner**  <br>*optional*|owner of the application|string|
|**siteURLs**  <br>*optional*|list of all site URLs|< string > array|
|**subscription**  <br>*optional*||boolean|
|**version**  <br>*optional*||string|


<a name="applicationidentifier"></a>
### ApplicationIdentifier

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*|unique identifier of the application identifier|string|
|**key**  <br>*optional*|identifier type|string|
|**name**  <br>*optional*|name of the identifier|string|
|**value**  <br>*optional*|list of identifier values|< string > array|


<a name="applicationtoken"></a>
### ApplicationToken

|Name|Schema|
|---|---|
|**apiAccessKey**  <br>*optional*|[APIAccessKey](#apiaccesskey)|
|**oauth2Token**  <br>*optional*|[OAuth2Token](#oauth2token)|


<a name="arraymodel"></a>
### ArrayModel
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**items**  <br>*optional*|[Property](#property)|
|**maxItems**  <br>*optional*|integer (int32)|
|**minItems**  <br>*optional*|integer (int32)|
|**properties**  <br>*optional*|< string, object > map|
|**reference**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="arrayproperty"></a>
### ArrayProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**items**  <br>*optional*|[Property](#property)|
|**maxItems**  <br>*optional*|integer (int32)|
|**minItems**  <br>*optional*|integer (int32)|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**uniqueItems**  <br>*optional*|boolean|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="arrayschema"></a>
### ArraySchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**items**  <br>*optional*||[Schema](#schema)|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="authorizationvalue"></a>
### AuthorizationValue

|Name|Schema|
|---|---|
|**keyName**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**value**  <br>*optional*|string|


<a name="baseintegerproperty"></a>
### BaseIntegerProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**exclusiveMaximum**  <br>*optional*|boolean|
|**exclusiveMinimum**  <br>*optional*|boolean|
|**format**  <br>*optional*|string|
|**maximum**  <br>*optional*|number|
|**minimum**  <br>*optional*|number|
|**multipleOf**  <br>*optional*|number|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="bodyparameter"></a>
### BodyParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*||< string, string > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**schema**  <br>*optional*||< string, [Model](#model) > map|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="booleanproperty"></a>
### BooleanProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**enum**  <br>*optional*|< boolean > array|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="booleanschema"></a>
### BooleanSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="callback"></a>
### Callback

|Name|Description|Schema|
|---|---|---|
|**callbacksMap**  <br>*optional*|A Path Item Object used to define a callback request and expected responses|< string, [Path](#path) > map|
|**get$ref**  <br>*optional*||string|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="commandinfo"></a>
### CommandInfo

|Name|Schema|
|---|---|
|**commandClass**  <br>*optional*|string|
|**commandName**  <br>*optional*|string|


<a name="components"></a>
### Components

|Name|Description|Schema|
|---|---|---|
|**callbacks**  <br>*optional*|An object to hold reusable callback objects|< string, [Callback](#callback) > map|
|**examples**  <br>*optional*|An object to hold reusable example objects|< string, [Example](#example) > map|
|**headers**  <br>*optional*|An object to hold reusable header objects|< string, [Header](#header) > map|
|**links**  <br>*optional*|An object to hold reusable link objects|< string, [Link](#link) > map|
|**parameters**  <br>*optional*|An object to hold reusable parameter objects|< string, [Parameter](#parameter) > map|
|**requestBodies**  <br>*optional*|An object to hold reusable requestBody objects|< string, [RequestBody](#requestbody) > map|
|**responses**  <br>*optional*|An object to hold reusable response objects|< string, [Response](#response) > map|
|**schemas**  <br>*optional*|An object to hold reusable schema objects|< string, [Schema](#schema) > map|
|**securitySchemes**  <br>*optional*|An object to hold reusable securityScheme objects|< string, [SecurityScheme](#securityscheme) > map|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="composedmodel"></a>
### ComposedModel
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**allOf**  <br>*optional*|< [Model](#model) > array|
|**anyOf**  <br>*optional*|< [Model](#model) > array|
|**child**  <br>*optional*|[Model](#model)|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**interfaces**  <br>*optional*|< [Model](#model) > array|
|**oneOf**  <br>*optional*|< [Model](#model) > array|
|**parent**  <br>*optional*|[Model](#model)|
|**properties**  <br>*optional*|< string, object > map|
|**reference**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="composedproperty"></a>
### ComposedProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allOf**  <br>*optional*|< [Property](#property) > array|
|**allowEmptyValue**  <br>*optional*|boolean|
|**anyOf**  <br>*optional*|< [Property](#property) > array|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**oneOf**  <br>*optional*|< [Property](#property) > array|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="composedschema"></a>
### ComposedSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**allOf**  <br>*optional*|Must be valid against all of the subschemas|< [Schema](#schema) > array|
|**anyOf**  <br>*optional*|Must be valid against any of the subschemas|< [Schema](#schema) > array|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**oneOf**  <br>*optional*|Must be valid against exactly one of the subschemas|< [Schema](#schema) > array|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*|It specifies the data type for a schema|string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="contact"></a>
### Contact

|Name|Description|Schema|
|---|---|---|
|**email**  <br>*optional*|The email address of the contact person/organization|string|
|**name**  <br>*optional*|The identifying name of the contact person/organization|string|
|**url**  <br>*optional*|The URL pointing to the contact information|string|


<a name="cookieparameter"></a>
### CookieParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="dataflavor"></a>
### DataFlavor

|Name|Schema|
|---|---|
|**defaultRepresentationClassAsString**  <br>*optional*|string|
|**flavorJavaFileListType**  <br>*optional*|boolean|
|**flavorRemoteObjectType**  <br>*optional*|boolean|
|**flavorSerializedObjectType**  <br>*optional*|boolean|
|**flavorTextType**  <br>*optional*|boolean|
|**humanPresentableName**  <br>*optional*|string|
|**mimeType**  <br>*optional*|string|
|**mimeTypeSerializedObject**  <br>*optional*|boolean|
|**primaryType**  <br>*optional*|string|
|**representationClassByteBuffer**  <br>*optional*|boolean|
|**representationClassCharBuffer**  <br>*optional*|boolean|
|**representationClassInputStream**  <br>*optional*|boolean|
|**representationClassReader**  <br>*optional*|boolean|
|**representationClassRemote**  <br>*optional*|boolean|
|**representationClassSerializable**  <br>*optional*|boolean|
|**subType**  <br>*optional*|string|


<a name="datahandler"></a>
### DataHandler

|Name|Schema|
|---|---|
|**allCommands**  <br>*optional*|< [CommandInfo](#commandinfo) > array|
|**content**  <br>*optional*|object|
|**contentType**  <br>*optional*|string|
|**dataSource**  <br>*optional*|[DataSource](#datasource)|
|**inputStream**  <br>*optional*|[InputStream](#inputstream)|
|**name**  <br>*optional*|string|
|**outputStream**  <br>*optional*|[OutputStream](#outputstream)|
|**preferredCommands**  <br>*optional*|< [CommandInfo](#commandinfo) > array|
|**transferDataFlavors**  <br>*optional*|< [DataFlavor](#dataflavor) > array|


<a name="datasource"></a>
### DataSource

|Name|Schema|
|---|---|
|**contentType**  <br>*optional*|string|
|**inputStream**  <br>*optional*|[InputStream](#inputstream)|
|**name**  <br>*optional*|string|
|**outputStream**  <br>*optional*|[OutputStream](#outputstream)|


<a name="datetimeproperty"></a>
### DateTimeProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**enum**  <br>*optional*|< object > array|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="decimalproperty"></a>
### DecimalProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**exclusiveMaximum**  <br>*optional*|boolean|
|**exclusiveMinimum**  <br>*optional*|boolean|
|**format**  <br>*optional*|string|
|**maximum**  <br>*optional*|number|
|**minimum**  <br>*optional*|number|
|**multipleOf**  <br>*optional*|number|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="endpoint"></a>
### Endpoint

|Name|Schema|
|---|---|
|**alias**  <br>*optional*|boolean|
|**connectionTimeoutDuration**  <br>*optional*|integer (int32)|
|**optimizationTechnique**  <br>*optional*|string|
|**passSecurityHeaders**  <br>*optional*|boolean|
|**uri**  <br>*optional*|string|


<a name="endpoints"></a>
### Endpoints
This defines the service registry publish information for API Gateway's API endpoints


|Name|Description|Schema|
|---|---|---|
|**gatewayEndpoint**  <br>*optional*|API's access endpoint exposed in API Gateway.|string|
|**status**  <br>*optional*|Status of the API endpoint. Shows whether this endpoint is published to the service registry.Possible values are NEW, PUBLISHED and SUSPENDED. NEW represents the endpoint is not published to the service registry. PUBLISHED represents the endpoint is published to the service registry. SUSPENDED represents the endpoint is published to service registry, but is not currently active (for example: during deactivation of API or shutdown of API Gateway or disabling ports).|enum (NEW, PUBLISHED, SUSPENDED)|


<a name="entityset"></a>
### EntitySet

|Name|Schema|
|---|---|
|**enabled**  <br>*optional*|boolean|
|**entityType**  <br>*optional*|string|
|**parameters**  <br>*optional*|< string, < string > array > map|


<a name="entitytype"></a>
### EntityType

|Name|Schema|
|---|---|
|**methods**  <br>*optional*|< string, [MethodParameters](#methodparameters) > map|
|**navigationProperties**  <br>*optional*|< string, [EntitySet](#entityset) > map|
|**properties**  <br>*optional*|< string, object > map|


<a name="enumeration"></a>
### Enumeration
*Type* : object


<a name="example"></a>
### Example

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|Long description for the example|string|
|**externalValue**  <br>*optional*|A URL that points to the literal example. This provides the capability to reference examples that cannot easily be included in JSON or YAML documents. The value field and externalValue field are mutually exclusive|string|
|**get$ref**  <br>*optional*||string|
|**summary**  <br>*optional*|Short description for the example|string|
|**value**  <br>*optional*|Embedded literal example. The value field and externalValue field are mutually exclusive. To represent examples of media types that cannot naturally represented in JSON or YAML, use a string value to contain the example, escaping where necessary|object|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="externaldocs"></a>
### ExternalDocs

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|A short description of the target documentation|string|
|**url**  <br>*optional*|The URL for the target documentation|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="formparameter"></a>
### FormParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="functionimport"></a>
### FunctionImport

|Name|Schema|
|---|---|
|**enabled**  <br>*optional*|boolean|
|**function**  <br>*optional*|string|


<a name="gatewayapi"></a>
### GatewayAPI

|Name|Schema|
|---|---|
|**apiDefinition**  <br>*optional*|[API](#api)|
|**apiDescription**  <br>*optional*|string|
|**apiDocuments**  <br>*optional*|< string > array|
|**apiEndpointPrefix**  <br>*optional*|string|
|**apiGroups**  <br>*optional*|< string > array|
|**apiName**  <br>*optional*|string|
|**apiVersion**  <br>*optional*|string|
|**centraSiteURL**  <br>*optional*  <br>*read-only*|string|
|**creationDate**  <br>*optional*  <br>*read-only*|string|
|**id**  <br>*optional*|string|
|**isActive**  <br>*optional*  <br>*read-only*|boolean|
|**lastModified**  <br>*optional*  <br>*read-only*|string|
|**maturityState**  <br>*optional*|string|
|**mockService**  <br>*optional*|[MockService](#mockservice)|
|**nativeEndpoint**  <br>*optional*  <br>*read-only*|< [Endpoint](#endpoint) > array|
|**nextVersion**  <br>*optional*  <br>*read-only*|string|
|**oauth2ScopeName**  <br>*optional*|string|
|**owner**  <br>*optional*  <br>*read-only*|string|
|**policies**  <br>*optional*  <br>*read-only*|< string > array|
|**prevVersion**  <br>*optional*  <br>*read-only*|string|
|**provider**  <br>*optional*  <br>*read-only*|string|
|**publishedPortals**  <br>*optional*|< string > array|
|**publishedToRegistry**  <br>*optional*|boolean|
|**rootFileName**  <br>*optional*  <br>*read-only*|string|
|**scopes**  <br>*optional*|< [Scope](#scope) > array|
|**systemVersion**  <br>*optional*  <br>*read-only*|integer (int32)|
|**type**  <br>*optional*|string|


<a name="gatewayschema"></a>
### GatewaySchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**schema**  <br>*optional*||string|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="header"></a>
### Header

|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="headerparameter"></a>
### HeaderParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="info"></a>
### Info

|Name|Description|Schema|
|---|---|---|
|**contact**  <br>*optional*||[Contact](#contact)|
|**description**  <br>*optional*|A short description of the application|string|
|**license**  <br>*optional*||[Licence](#licence)|
|**termsOfService**  <br>*optional*|A URL to the Terms of Service for the API|string|
|**title**  <br>*optional*|The title of the application|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**version**  <br>*optional*|Version of the API|string|


<a name="inputapi"></a>
### InputAPI

|Name|Description|Schema|
|---|---|---|
|**apiDefinition**  <br>*optional*||[API](#api)|
|**apiDescription**  <br>*optional*||string|
|**apiName**  <br>*required*||string|
|**apiVersion**  <br>*optional*||string|
|**authorizationValue**  <br>*optional*||[AuthorizationValue](#authorizationvalue)|
|**maturityState**  <br>*optional*||string|
|**rootFileName**  <br>*optional*|Required when creating an API by importing protected URL|string|
|**teams**  <br>*optional*|Contains teams to which the API must be assigned.|< [Team](#team) > array|
|**type**  <br>*required*||string|
|**url**  <br>*optional*|Required when creating an API by importing URL|string|


<a name="inputintegrationserverpublish"></a>
### InputIntegrationServerPublish

|Name|Description|Schema|
|---|---|---|
|**publishInfo**  <br>*optional*||[PublishPayload](#publishpayload)|
|**publishInfos**  <br>*optional*|This contains the publish information for multiple APIs. Required when publishing more than one API to one or more integration servers.|< [PublishPayload](#publishpayload) > array|


<a name="inputpublish"></a>
### InputPublish

|Name|Schema|
|---|---|
|**communities**  <br>*optional*|< string > array|
|**endpoints**  <br>*optional*|< string > array|
|**portalGatewayId**  <br>*optional*|string|


<a name="inputserviceregistrypublish"></a>
### InputServiceRegistryPublish

|Name|Description|Schema|
|---|---|---|
|**publishInfo**  <br>*optional*||[PublishPayload](#publishpayload)|
|**publishInfos**  <br>*optional*|This contains the publish information for multiple APIs. Required when publishing more than one API to one or more service registries.|< [PublishPayload](#publishpayload) > array|


<a name="inputserviceregistryunpublish"></a>
### InputServiceRegistryUnpublish

|Name|Description|Schema|
|---|---|---|
|**unpublishInfo**  <br>*optional*||[UnpublishInfo](#unpublishinfo)|
|**unpublishInfos**  <br>*optional*|This contains the unpublish information for multiple APIs. Required when publishing more than one API from one or more service registries.|< [UnpublishInfo](#unpublishinfo) > array|


<a name="inputstream"></a>
### InputStream
*Type* : object


<a name="inputversion"></a>
### InputVersion

|Name|Schema|
|---|---|
|**newApiVersion**  <br>*required*|string|
|**retainApplications**  <br>*optional*|boolean|


<a name="integerschema"></a>
### IntegerSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*||[Number](#number)|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< [Number](#number) > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*||string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="integrationserverpublishinfo"></a>
### IntegrationServerPublishInfo

|Name|Description|Schema|
|---|---|---|
|**folderName**  <br>*optional*|Name of the folder under the package (mentioned on 'packageName' property) in which the API to be published. This field is required.|string|
|**integrationServerId**  <br>*optional*|Uddi key of the integration server created in API Gateway. This field is required.|string|
|**integrationServerName**  <br>*optional*||string|
|**packageName**  <br>*optional*|Name of the package in the integration server in which the API to be published. This field is required.|string|
|**status**  <br>*optional*||enum (NEW, PUBLISHED, SUSPENDED)|


<a name="integrationserverpublishresponse"></a>
### IntegrationServerPublishResponse

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|Represents the status of the publish operation of the API to the service registry eg: Publish successful, Publish failed, etc|string|
|**failureReason**  <br>*optional*|Provides the reason for the failure when the publish operation is not successful|string|
|**integrationServerId**  <br>*optional*|Id i.e, UDDI key of the service registry|string|
|**integrationServerName**  <br>*optional*||string|
|**status**  <br>*optional*||enum (NEW, PUBLISHED, SUSPENDED)|
|**success**  <br>*optional*|Represents whether the publish of API to the service registry is success. Possible values: true/false|boolean|


<a name="licence"></a>
### Licence

|Name|Description|Schema|
|---|---|---|
|**name**  <br>*optional*|The license name used for the API|string|
|**url**  <br>*optional*|A URL to the license used for the API|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="link"></a>
### Link

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|A description of the link|string|
|**get$ref**  <br>*optional*||string|
|**operationId**  <br>*optional*|The name of an existing, resolvable OAS operation, as defined with a unique operationId. This field is mutually exclusive of the operationRef field|string|
|**operationRef**  <br>*optional*|A relative or absolute reference to an OAS operation. This field is mutually exclusive of the operationId field, and MUST point to an Operation Object. Relative operationRef values MAY be used to locate an existing Operation Object in the API definition|string|
|**parameters**  <br>*optional*|A map representing parameters to pass to an operation as specified with operationId or identified via operationRef. The key is the parameter name to be used, whereas the value can be a constant or an expression to be evaluated and passed to the linked operation. The parameter name can be qualified using the parameter location [{in}.]{name} for operations that use the same parameter name in different locations (e.g. path.id)|< string, string > map|
|**requestBody**  <br>*optional*|A literal value or {expression} to use as a request body when calling the target operation|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="mapproperty"></a>
### MapProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**additionalProperties**  <br>*optional*|[Property](#property)|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**maxProperties**  <br>*optional*|integer (int32)|
|**minProperties**  <br>*optional*|integer (int32)|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="mediatype"></a>
### MediaType

|Name|Description|Schema|
|---|---|---|
|**example**  <br>*optional*|Example of the media type. The example object SHOULD be in the correct format as specified by the media type. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema|object|
|**examples**  <br>*optional*|Examples of the media type. Each example object SHOULD match the media type and specified schema if present. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**schema**  <br>*optional*||[Schema](#schema)|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="messageframe"></a>
### MessageFrame

|Name|Schema|
|---|---|
|**messageDescription**  <br>*optional*|string|
|**messagePayload**  <br>*optional*|string|
|**origin**  <br>*optional*|enum (Server, Client)|
|**type**  <br>*optional*|enum (Binary, Text)|


<a name="methodparameters"></a>
### MethodParameters

|Name|Schema|
|---|---|
|**enabled**  <br>*optional*|boolean|
|**parameters**  <br>*optional*|< string, string > map|
|**returnType**  <br>*optional*|string|


<a name="mockservice"></a>
### MockService

|Name|Schema|
|---|---|
|**enableMock**  <br>*optional*|boolean|
|**runAsUser**  <br>*optional*|string|
|**service**  <br>*optional*|string|


<a name="mockedcondition"></a>
### MockedCondition

|Name|Schema|
|---|---|
|**conditionName**  <br>*optional*|string|
|**mockedConditionParameter**  <br>*optional*|enum (Body, Header, QueryParameter)|
|**mockedLevel1Operator**  <br>*optional*|enum (Equals, NotEquals, ContainsKey, ContainsKeyValue)|
|**mockedLevel2Operator**  <br>*optional*|enum (Equals, NotEquals, Contains, StartsWith, EndsWith)|
|**value1**  <br>*optional*|string|
|**value2**  <br>*optional*|string|


<a name="mockedconditionsbasedcustomresponse"></a>
### MockedConditionsBasedCustomResponse

|Name|Schema|
|---|---|
|**mockedConditionList**  <br>*optional*|< [MockedCondition](#mockedcondition) > array|
|**mockedResponse**  <br>*optional*|[MockedResponse](#mockedresponse)|


<a name="mockedresponse"></a>
### MockedResponse

|Name|Schema|
|---|---|
|**responseBody**  <br>*optional*|< string, object > map|
|**responseHeaders**  <br>*optional*|< string, object > map|
|**statusCode**  <br>*optional*|string|


<a name="model"></a>
### Model

|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**properties**  <br>*optional*|< string, object > map|
|**reference**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="modelimpl"></a>
### ModelImpl
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**allowEmptyValue**  <br>*optional*|boolean|
|**defaultValue**  <br>*optional*|object|
|**description**  <br>*optional*|string|
|**discriminator**  <br>*optional*|string|
|**enum**  <br>*optional*|< string > array|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|string|
|**maximum**  <br>*optional*|number|
|**minimum**  <br>*optional*|number|
|**name**  <br>*optional*|string|
|**properties**  <br>*optional*  <br>*read-only*|< string, [Property](#property) > map|
|**reference**  <br>*optional*|string|
|**required**  <br>*optional*|< string > array|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**uniqueItems**  <br>*optional*|boolean|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="multipart"></a>
### Multipart

|Name|Schema|
|---|---|
|**contentType**  <br>*optional*|string|
|**count**  <br>*optional*|integer (int32)|
|**parent**  <br>*optional*|[Part](#part)|


<a name="namespaces"></a>
### Namespaces

|Name|Schema|
|---|---|
|**prefix**  <br>*optional*|string|
|**uri**  <br>*optional*|string|


<a name="number"></a>
### Number
*Type* : object


<a name="numberschema"></a>
### NumberSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|number|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< number > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="oauth2definition"></a>
### OAuth2Definition

|Name|Schema|
|---|---|
|**authorizationGrants**  <br>*optional*|< string > array|
|**authorizationUrl**  <br>*optional*|string|
|**description**  <br>*optional*|string|
|**flow**  <br>*optional*|string|
|**refreshUrl**  <br>*optional*|string|
|**scopes**  <br>*optional*|< string, object > map|
|**securitySchemeDescriptor**  <br>*optional*|[SecuritySchemeDescriptor](#securityschemedescriptor)|
|**tokenUrl**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="oauth2token"></a>
### OAuth2Token

|Name|Description|Schema|
|---|---|---|
|**clientId**  <br>*optional*|unique identifier of the oauth2 client|string|
|**clientName**  <br>*optional*|the name of the client|string|
|**clientSecret**  <br>*optional*|the client secret|string|
|**expirationInterval**  <br>*optional*|the expiration interval|string|
|**redirectUris**  <br>*optional*|list of redirect uris|< string > array|
|**refreshCount**  <br>*optional*|number of times an access token can be refreshed|string|
|**scopes**  <br>*optional*|the scopes associated with the client|< string > array|
|**type**  <br>*optional*|type of the oauth2 client|string|


<a name="oauthflows"></a>
### OAuthFlows

|Name|Schema|
|---|---|
|**authorizationCode**  <br>*optional*|[OAuth2Definition](#oauth2definition)|
|**clientCredentials**  <br>*optional*|[OAuth2Definition](#oauth2definition)|
|**implicit**  <br>*optional*|[OAuth2Definition](#oauth2definition)|
|**password**  <br>*optional*|[OAuth2Definition](#oauth2definition)|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="odataapi"></a>
### ODataAPI
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**actionImports**  <br>*optional*||< string, [ActionImport](#actionimport) > map|
|**actions**  <br>*optional*||< string, [EntityType](#entitytype) > map|
|**apiTags**  <br>*optional*||< string > array|
|**description**  <br>*optional*||string|
|**entitySets**  <br>*optional*||< string, [EntitySet](#entityset) > map|
|**entityTypes**  <br>*optional*||< string, [EntityType](#entitytype) > map|
|**functionImports**  <br>*optional*||< string, [FunctionImport](#functionimport) > map|
|**functions**  <br>*optional*||< string, [EntityType](#entitytype) > map|
|**metaDataDocument**  <br>*optional*||string|
|**odataVersion**  <br>*optional*||string|
|**serviceDocument**  <br>*optional*||string|
|**serviceRegistryDisplayName**  <br>*optional*|The name of the API in service registry when the API is published to a service registry.|string|
|**serviceRoot**  <br>*optional*||string|
|**singletons**  <br>*optional*||< string, [EntitySet](#entityset) > map|
|**tags**  <br>*optional*||< [Tag](#tag) > array|
|**title**  <br>*optional*||string|
|**type**  <br>*optional*||string|
|**uri**  <br>*optional*||< string > array|
|**version**  <br>*optional*||string|


<a name="objectproperty"></a>
### ObjectProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**properties**  <br>*optional*|< string, [Property](#property) > map|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="objectschema"></a>
### ObjectSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="operation"></a>
### Operation

|Name|Description|Schema|
|---|---|---|
|**callbacks**  <br>*optional*|An optional, string description, intended to apply to all operations in this path|< string, [Callback](#callback) > map|
|**deprecated**  <br>*optional*|Declares this operation to be deprecated. Consumers SHOULD refrain from usage of the declared operation. Default value is false|boolean|
|**description**  <br>*optional*|A verbose explanation of the operation behavior|string|
|**enabled**  <br>*optional*||boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**mockedConditionsBasedCustomResponsesList**  <br>*optional*|The list of mocked conditions and it's applicable only for mocked APIs|< [MockedConditionsBasedCustomResponse](#mockedconditionsbasedcustomresponse) > array|
|**mockedResponses**  <br>*optional*|The list of possible mocked responses as they are returned from executing this operation and it's applicable only for mocked APIs|< string, [MockedResponse](#mockedresponse) > map|
|**operationId**  <br>*optional*|Unique string used to identify the operation. The id MUST be unique among all operations described in the API. The operationId value is case-sensitive|string|
|**parameters**  <br>*optional*|A list of parameters that are applicable for this operation. If a parameter is already defined at the Path Item, the new definition will override it but can never remove it|< [Parameter](#parameter) > array|
|**requestBody**  <br>*optional*||[RequestBody](#requestbody)|
|**responses**  <br>*optional*|The list of possible responses as they are returned from executing this operation|< string, [Response](#response) > map|
|**scopes**  <br>*optional*||< string > array|
|**summary**  <br>*optional*|A short summary of what the operation does|string|
|**tags**  <br>*optional*|A list of tags for API documentation control. Tags can be used for logical grouping of operations by resources or any other qualifier|< string > array|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="outputstream"></a>
### OutputStream
*Type* : object


<a name="parameter"></a>
### Parameter

|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="part"></a>
### Part

|Name|Schema|
|---|---|
|**allHeaders**  <br>*optional*|[Enumeration](#enumeration)|
|**content**  <br>*optional*|object|
|**contentType**  <br>*optional*|string|
|**dataHandler**  <br>*optional*|[DataHandler](#datahandler)|
|**description**  <br>*optional*|string|
|**disposition**  <br>*optional*|string|
|**fileName**  <br>*optional*|string|
|**inputStream**  <br>*optional*|[InputStream](#inputstream)|
|**lineCount**  <br>*optional*|integer (int32)|
|**size**  <br>*optional*|integer (int32)|


<a name="path"></a>
### Path

|Name|Description|Schema|
|---|---|---|
|**delete**  <br>*optional*||[Operation](#operation)|
|**description**  <br>*optional*|An optional, string description, intended to apply to all operations in this path|string|
|**displayName**  <br>*optional*||string|
|**enabled**  <br>*optional*||boolean|
|**get**  <br>*optional*||[Operation](#operation)|
|**get$ref**  <br>*optional*|Allows for an external definition of this path item|string|
|**head**  <br>*optional*||[Operation](#operation)|
|**options**  <br>*optional*||[Operation](#operation)|
|**parameters**  <br>*optional*|A list of parameters that are applicable for all the operations described under this path. These parameters can be overridden at the operation level, but cannot be removed there|< object > array|
|**patch**  <br>*optional*||[Operation](#operation)|
|**post**  <br>*optional*||[Operation](#operation)|
|**put**  <br>*optional*||[Operation](#operation)|
|**scopes**  <br>*optional*||< string > array|
|**summary**  <br>*optional*|An optional, string summary, intended to apply to all operations in this path|string|
|**tags**  <br>*optional*||< string > array|
|**trace**  <br>*optional*||[Operation](#operation)|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="pathparameter"></a>
### PathParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="property"></a>
### Property

|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="publishpayload"></a>
### PublishPayload

|Name|Description|Schema|
|---|---|---|
|**apiId**  <br>*optional*|API id for the API to be published. This field is required. The API will be published to the service registry with the value configured in 'Service registry display name' field of the API|string|
|**integrationServerPublishInfo**  <br>*optional*||< [IntegrationServerPublishInfo](#integrationserverpublishinfo) > array|
|**serviceRegistryPublishInfo**  <br>*optional*|List of service registry publish information for the API. Each element of the list contains the publish information of the API for one service registry.|< [ServiceRegistryPublishInfo](#serviceregistrypublishinfo) > array|


<a name="publishresponse"></a>
### PublishResponse

|Name|Description|Schema|
|---|---|---|
|**apiId**  <br>*optional*|API id of the API published.|string|
|**apiName**  <br>*optional*|API name of the API published.|string|
|**apiVersion**  <br>*optional*|API version of the API published.|string|
|**integrationServerPublishResponses**  <br>*optional*||< [IntegrationServerPublishResponse](#integrationserverpublishresponse) > array|
|**serviceRegistryPublishResponses**  <br>*optional*|Contains publish status of the API for each service registry in the request.|< [ServiceRegistryPublishResponse](#serviceregistrypublishresponse) > array|


<a name="queryparameter"></a>
### QueryParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="refmodel"></a>
### RefModel
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**get$ref**  <br>*optional*|string|
|**properties**  <br>*optional*|< string, object > map|
|**refFormat**  <br>*optional*|enum (URL, RELATIVE, INTERNAL)|
|**reference**  <br>*optional*|string|
|**simpleRef**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="refparameter"></a>
### RefParameter
*Polymorphism* : Composition


|Name|Description|Schema|
|---|---|---|
|**_enum**  <br>*optional*||< string > array|
|**allowEmptyValue**  <br>*optional*|Sets the ability to pass empty-valued parameters. This is valid only for query parameters and allows sending a parameter with an empty value|boolean|
|**allowReserved**  <br>*optional*|Determines whether the parameter value SHOULD allow reserved characters, as defined by RFC3986 :/?#[]@!$&'()*+,;= to be included without percent-encoding. This property only applies to parameters with an in value of query. The default value is false|boolean|
|**content**  <br>*optional*|A map containing the representations for the parameter. The key is the media type and the value describes it. The map MUST only contain one entry|< string, [MediaType](#mediatype) > map|
|**default**  <br>*optional*||string|
|**deprecated**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**description**  <br>*optional*|A brief description of the parameter. This could contain examples of use|string|
|**examples**  <br>*optional*|Examples of the media type. Each example SHOULD contain a value in the correct format as specified in the parameter encoding. The examples field is mutually exclusive of the example field. Furthermore, if referencing a schema which contains an example, the examples value SHALL override the example provided by the schema|< string, [Example](#example) > map|
|**explode**  <br>*optional*|When this is true, parameter values of type array or object generate separate parameters for each value of the array or key-value pair of the map. For other types of parameters this property has no effect. When style is form, the default value is true. For all other styles, the default value is false|boolean|
|**extendedExample**  <br>*optional*|Example of the media type. The example SHOULD match the specified schema and encoding properties if present. The example field is mutually exclusive of the examples field. Furthermore, if referencing a schema which contains an example, the example value SHALL override the example provided by the schema. To represent examples of media types that cannot naturally be represented in JSON or YAML, a string value can contain the example with escaping where necessary|object|
|**get$ref**  <br>*optional*|The available paths and operations for the API|string|
|**in**  <br>*optional*|The location of the parameter. Possible values are "query", "header", "path" or "cookie"|string|
|**name**  <br>*optional*|The name of the parameter. Parameter names are case sensitive|string|
|**parameterSchema**  <br>*optional*||[Schema](#schema)|
|**required**  <br>*optional*|Determines whether this parameter is mandatory. If the parameter location is "path", this property is REQUIRED and its value MUST be true. Otherwise, the property MAY be included and its default value is false|boolean|
|**style**  <br>*optional*|Describes how the parameter value will be serialized depending on the type of the parameter value. Default values (based on value of in): for query - form; for path - simple; for header - simple; for cookie - form|enum (MATRIX, LABEL, FORM, SIMPLE, SPACEDELIMITED, PIPEDELIMITED, DEEPOBJECT)|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**xpath**  <br>*optional*||[Xpath](#xpath)|


<a name="refproperty"></a>
### RefProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**get$ref**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**refFormat**  <br>*optional*|enum (URL, RELATIVE, INTERNAL)|
|**required**  <br>*optional*|boolean|
|**simpleRef**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="requestbody"></a>
### RequestBody

|Name|Description|Schema|
|---|---|---|
|**content**  <br>*optional*|The content of the request body. The key is a media type or media type range and the value describes it|< string, [MediaType](#mediatype) > map|
|**description**  <br>*optional*|A brief description of the request body. This could contain examples of use|string|
|**get$ref**  <br>*optional*||string|
|**required**  <br>*optional*|Determines if the request body is required in the request. Defaults to false|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="response"></a>
### Response

|Name|Description|Schema|
|---|---|---|
|**content**  <br>*optional*|A map containing descriptions of potential response payloads. The key is a media type or media type range and the value describes it|< string, [MediaType](#mediatype) > map|
|**description**  <br>*optional*|A short description of the response|string|
|**get$ref**  <br>*optional*||string|
|**headersV3**  <br>*optional*|Maps a header name to its definition. RFC7230 states header names are case insensitive. If a response header is defined with the name "Content-Type", it SHALL be ignored|< string, [Header](#header) > map|
|**links**  <br>*optional*|A map of operations links that can be followed from the response. The key of the map is a short name for the link, following the naming constraints of the names for Component Objects.|< string, [Link](#link) > map|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="restapi"></a>
### RestAPI
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**apiTags**  <br>*optional*||< string > array|
|**components**  <br>*optional*||[Components](#components)|
|**description**  <br>*optional*||string|
|**externalDocs**  <br>*optional*|Additional external documentation|< [ExternalDocs](#externaldocs) > array|
|**info**  <br>*optional*||[Info](#info)|
|**paths**  <br>*optional*|The available paths and operations for the API|< string, [Path](#path) > map|
|**servers**  <br>*optional*|An array of Server Objects, which provide connectivity information to a target server|< [Server](#server) > array|
|**serviceRegistryDisplayName**  <br>*optional*|The name of the API in service registry when the API is published to a service registry.|string|
|**tags**  <br>*optional*|A list of tags with additional metadata|< [Tag](#tag) > array|
|**title**  <br>*optional*||string|
|**type**  <br>*optional*||string|
|**uri**  <br>*optional*||< string > array|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**version**  <br>*optional*||string|


<a name="restenabledpath"></a>
### RestEnabledPath

|Name|Description|Schema|
|---|---|---|
|**delete**  <br>*optional*||[Operation](#operation)|
|**description**  <br>*optional*|An optional, string description, intended to apply to all operations in this path|string|
|**displayName**  <br>*optional*||string|
|**enabled**  <br>*optional*||boolean|
|**get**  <br>*optional*||[Operation](#operation)|
|**get$ref**  <br>*optional*|Allows for an external definition of this path item|string|
|**head**  <br>*optional*||[Operation](#operation)|
|**invokePath**  <br>*optional*||string|
|**name**  <br>*optional*||string|
|**options**  <br>*optional*||[Operation](#operation)|
|**parameters**  <br>*optional*|A list of parameters that are applicable for all the operations described under this path. These parameters can be overridden at the operation level, but cannot be removed there|< object > array|
|**patch**  <br>*optional*||[Operation](#operation)|
|**post**  <br>*optional*||[Operation](#operation)|
|**put**  <br>*optional*||[Operation](#operation)|
|**scopes**  <br>*optional*||< string > array|
|**summary**  <br>*optional*|An optional, string summary, intended to apply to all operations in this path|string|
|**tags**  <br>*optional*||< string > array|
|**trace**  <br>*optional*||[Operation](#operation)|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="soapapi"></a>
### SOAPAPI
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**apiTags**  <br>*optional*||< string > array|
|**baseWsdlUri**  <br>*optional*||string|
|**description**  <br>*optional*||string|
|**isRESTInvokeEnabled**  <br>*optional*||boolean|
|**nativeUri**  <br>*optional*||< string > array|
|**operationPolicies**  <br>*optional*||< string, string > map|
|**operationsInfo**  <br>*optional*||< [SOAPOperation](#soapoperation) > array|
|**primaryEndpoint**  <br>*optional*||< string, boolean > map|
|**rootFileFolder**  <br>*optional*||string|
|**serviceName**  <br>*optional*||string|
|**serviceRegistryDisplayName**  <br>*optional*|The name of the API in service registry when the API is published to a service registry.|string|
|**tags**  <br>*optional*||< [Tag](#tag) > array|
|**title**  <br>*optional*||string|
|**type**  <br>*optional*||string|
|**uri**  <br>*optional*||< string > array|
|**version**  <br>*optional*||string|
|**wsdl**  <br>*optional*||string|


<a name="soapbinding"></a>
### SOAPBinding

|Name|Schema|
|---|---|
|**faultMessages**  <br>*optional*|< string > array|
|**inputMessage**  <br>*optional*|string|
|**interFace**  <br>*optional*|[SOAPInterface](#soapinterface)|
|**name**  <br>*optional*|string|
|**outputMessage**  <br>*optional*|string|
|**specifier**  <br>*optional*|string|
|**type**  <br>*optional*|string|


<a name="soapinterface"></a>
### SOAPInterface

|Name|Schema|
|---|---|
|**name**  <br>*optional*|string|
|**operations**  <br>*optional*|< [SOAPOperation](#soapoperation) > array|


<a name="soapoperation"></a>
### SOAPOperation

|Name|Schema|
|---|---|
|**bindings**  <br>*optional*|< [SOAPBinding](#soapbinding) > array|
|**defined**  <br>*optional*|boolean|
|**enabled**  <br>*optional*|boolean|
|**isRESTInvokeEnabled**  <br>*optional*|boolean|
|**mockedConditionsBasedCustomResponsesList**  <br>*optional*|< [MockedConditionsBasedCustomResponse](#mockedconditionsbasedcustomresponse) > array|
|**mockedResponses**  <br>*optional*|< string, [MockedResponse](#mockedresponse) > map|
|**name**  <br>*optional*|string|
|**namespace**  <br>*optional*|string|
|**restEnabledPath**  <br>*optional*|[RestEnabledPath](#restenabledpath)|
|**scopes**  <br>*optional*|< string > array|
|**soapAction**  <br>*optional*|string|
|**tags**  <br>*optional*|< string > array|


<a name="schema"></a>
### Schema

|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|object|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< object > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*|It specifies the data type for a schema|string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="scope"></a>
### Scope

|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**getoAuth2ScopeName**  <br>*optional*|string|
|**mashedUpAPI**  <br>*optional*|boolean|
|**name**  <br>*optional*|string|
|**policies**  <br>*optional*|< string > array|


<a name="scopeinformation"></a>
### ScopeInformation

|Name|Schema|
|---|---|
|**resourcePath**  <br>*optional*|string|
|**supportedOperations**  <br>*optional*|< string > array|


<a name="scoperesourceindex"></a>
### ScopeResourceIndex

|Name|Schema|
|---|---|
|**references**  <br>*optional*|< [ScopeInformation](#scopeinformation) > array|
|**scope**  <br>*optional*|[Scope](#scope)|


<a name="securityscheme"></a>
### SecurityScheme

|Name|Schema|
|---|---|
|**bearerFormat**  <br>*optional*|string|
|**description**  <br>*optional*|string|
|**flows**  <br>*optional*|[OAuthFlows](#oauthflows)|
|**get$ref**  <br>*optional*|string|
|**in**  <br>*optional*|enum (COOKIE, HEADER, QUERY)|
|**name**  <br>*optional*|string|
|**openIdConnectUrl**  <br>*optional*|string|
|**scheme**  <br>*optional*|string|
|**type**  <br>*optional*|enum (APIKEY, HTTP, OAUTH2, OPENIDCONNECT)|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="securityschemedescriptor"></a>
### SecuritySchemeDescriptor

|Name|Schema|
|---|---|
|**headers**  <br>*optional*|< string, object > map|
|**queryParameters**  <br>*optional*|< string, object > map|
|**responses**  <br>*optional*|< string, object > map|


<a name="server"></a>
### Server

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|An optional string describing the host designated by the URL|string|
|**url**  <br>*optional*|A URL to the target host. This URL supports Server Variables and MAY be relative, to indicate that the host location is relative to the location where the OpenAPI document is being served. Variable substitutions will be made when a variable is named in {brackets}|string|
|**variables**  <br>*optional*|A map between a variable name and its value. The value is used for substitution in the server's URL template|< string, [ServerVariable](#servervariable) > map|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="servervariable"></a>
### ServerVariable

|Name|Description|Schema|
|---|---|---|
|**default**  <br>*optional*||string|
|**description**  <br>*optional*|An optional description for the server variable|string|
|**enum**  <br>*optional*||< string > array|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="serviceregistrypublishgetresponse"></a>
### ServiceRegistryPublishGetResponse

|Name|Schema|
|---|---|
|**publishInfo**  <br>*optional*|[PublishPayload](#publishpayload)|


<a name="serviceregistrypublishinfo"></a>
### ServiceRegistryPublishInfo

|Name|Description|Schema|
|---|---|---|
|**gatewayEndpoints**  <br>*optional*|List of API endpoints of the API. Each element contains an endpoint and the information about the publish status of that endpoint for the current service registry.|< [Endpoints](#endpoints) > array|
|**name**  <br>*optional*|Name of the service registry. This field is shown only in response and should not be sent by clients in requests. Only the serviceRegistryId is considered for uniquely identifying the registry.|string|
|**serviceRegistryId**  <br>*optional*|Uddi key of the service registry created in API Gateway. This field is required.|string|
|**status**  <br>*optional*|Publish Status of the API for this service registry. This field is shown only in response and should not be sent by clients in requests. Possible values are NEW, PUBLISHED and SUSPENDED. NEW represents the API is not published to the service registry. PUBLISHED represents the API is published to the service registry. SUSPENDED represents the API is published to service registry, but is not currently active (during deactivation of API or shutdown of API Gateway).|enum (NEW, PUBLISHED, SUSPENDED)|


<a name="serviceregistrypublishputresponse"></a>
### ServiceRegistryPublishPutResponse

|Name|Description|Schema|
|---|---|---|
|**publishResponse**  <br>*optional*||[PublishResponse](#publishresponse)|
|**publishResponses**  <br>*optional*|This contains the service registry publish status for requests publishing more than one API to one or more service registries.|< [PublishResponse](#publishresponse) > array|


<a name="serviceregistrypublishresponse"></a>
### ServiceRegistryPublishResponse

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|Represents the status of the publish operation of the API to the service registry eg: Publish successful, Publish failed, etc|string|
|**failureReason**  <br>*optional*|Provides the reason for the failure when the publish operation is not successful|string|
|**gatewayEndpoints**  <br>*optional*|List of API endpoints of the API. Each element contains an endpoint and the information about the publish status of that endpoint for the current service registry.|< [Endpoints](#endpoints) > array|
|**serviceRegistryId**  <br>*optional*|Id i.e, UDDI key of the service registry|string|
|**serviceRegistryName**  <br>*optional*|Name of the service registry|string|
|**status**  <br>*optional*|Publish Status of the API for this service registry. Possible values are NEW, PUBLISHED and SUSPENDED. NEW represents the API is not published to the service registry. PUBLISHED represents the API is published to the service registry. SUSPENDED represents the API is published to service registry, but is not currently active (during deactivation of API or shutdown of API Gateway).|enum (NEW, PUBLISHED, SUSPENDED)|
|**success**  <br>*optional*|Represents whether the publish of API to the service registry is success. Possible values: true/false|boolean|


<a name="serviceregistryunpublishputresponse"></a>
### ServiceRegistryUnpublishPutResponse

|Name|Description|Schema|
|---|---|---|
|**unpublishResponse**  <br>*optional*||[UnpublishResponse](#unpublishresponse)|
|**unpublishResponses**  <br>*optional*|This contains the service registry unpublish status for requests unpublishing more than one API from one or more service registries.|< [UnpublishResponse](#unpublishresponse) > array|


<a name="serviceregistryunpublishresponse"></a>
### ServiceRegistryUnpublishResponse

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|Represents the status of the unpublish operation of the API from the service registry eg: Unpublish successful, Unpublish failed, etc|string|
|**failureReason**  <br>*optional*|Provides the reason for the failure when the unpublish operation is not successful|string|
|**serviceRegistryId**  <br>*optional*|Id i.e, UDDI key of the service registry|string|
|**serviceRegistryName**  <br>*optional*|Name of the service registry|string|
|**success**  <br>*optional*|Represents whether the unpublish operation of API from the service registry is success. Possible values: true/false|boolean|


<a name="stringproperty"></a>
### StringProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**default**  <br>*optional*|string|
|**description**  <br>*optional*|string|
|**enum**  <br>*optional*|< string > array|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**maxLength**  <br>*optional*|integer (int32)|
|**minLength**  <br>*optional*|integer (int32)|
|**name**  <br>*optional*|string|
|**pattern**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="stringschema"></a>
### StringSchema
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Description|Schema|
|---|---|---|
|**additionalProperties**  <br>*optional*||[Schema](#schema)|
|**default**  <br>*optional*|The default value represents what would be assumed by the consumer of the input as the value of the schema if one is not provided. Unlike JSON Schema, the value MUST conform to the defined type for the Schema Object defined at the same level. For example, if type is string, then default can be "foo" but cannot be 1|string|
|**deprecated**  <br>*optional*|Specifies that a schema is deprecated and SHOULD be transitioned out of usage. Default value is false|boolean|
|**description**  <br>*optional*|Provide a more lengthy explanation about the purpose of the data described by the schema|string|
|**enum**  <br>*optional*||< string > array|
|**example**  <br>*optional*|A free-form property to include an example of an instance for this schema. To represent examples that cannot be naturally represented in JSON or YAML, a string value can be used to contain the example with escaping where necessary|object|
|**exclusiveMaximum**  <br>*optional*|Indicate whether maximum are exclusive of the value|boolean|
|**exclusiveMinimum**  <br>*optional*|Indicate whether minimum are exclusive of the value|boolean|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**format**  <br>*optional*|The format keyword allows for basic semantic validation on certain kinds of string values that are commonly used|string|
|**get$ref**  <br>*optional*||string|
|**maxItems**  <br>*optional*|The maximum length of the array can be specified|integer (int32)|
|**maxLength**  <br>*optional*|The maximum length of a string can be constrained using the minLength|integer (int32)|
|**maxProperties**  <br>*optional*|The maximum number of properties on an object can be restricted|integer (int32)|
|**maximum**  <br>*optional*|Upper limit in the ranges of numbers, (or exclusiveMinimum and exclusiveMaximum for expressing exclusive range)|number|
|**minItems**  <br>*optional*|The minimum length of the array can be specified|integer (int32)|
|**minLength**  <br>*optional*|The minimum length of a string can be constrained using the minLength|integer (int32)|
|**minProperties**  <br>*optional*|The minimum number of properties on an object can be restricted|integer (int32)|
|**minimum**  <br>*optional*|Lower limit in the ranges of numbers|number|
|**multipleOf**  <br>*optional*|Numbers can be restricted to a multiple of a given number, using the multipleOf keyword. It may be set to any positive number.|number|
|**name**  <br>*optional*|User defined name for the property|string|
|**not**  <br>*optional*||[Schema](#schema)|
|**nullable**  <br>*optional*|Allows sending a null value for the defined schema. Default value is false|boolean|
|**pattern**  <br>*optional*|The pattern keyword is used to restrict a string to a particular regular expression. The regular expression syntax is the one defined in JavaScript (ECMA 262 specifically)|string|
|**properties**  <br>*optional*|The properties (key-value pairs) on an object are defined using the properties keyword. The value of properties is an object, where each key is the name of a property and each value is of type schema used to validate that property|< string, [Schema](#schema) > map|
|**readOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "read only". This means that it MAY be sent as part of a response but SHOULD NOT be sent as part of the request. If the property is marked as readOnly being true and is in the required list, the required will take effect on the response only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**required**  <br>*optional*|By default, the properties defined by the properties keyword are not required. However, one can provide a list of required properties using the required keyword.<br>The required keyword takes an array of zero or more strings. Each of these strings must be unique.|< string > array|
|**title**  <br>*optional*|User defined title for the property|string|
|**type**  <br>*optional*||string|
|**uniqueItems**  <br>*optional*|A schema can ensure that each of the items in an array is unique. Simply set the uniqueItems keyword to true|boolean|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**writeOnly**  <br>*optional*|Relevant only for Schema "properties" definitions. Declares the property as "write only". Therefore, it MAY be sent as part of a request but SHOULD NOT be sent as part of the response. If the property is marked as writeOnly being true and is in the required list, the required will take effect on the request only. A property MUST NOT be marked as both readOnly and writeOnly being true. Default value is false|boolean|
|**xml**  <br>*optional*||[Xml](#xml)|


<a name="stringschemamodel"></a>
### StringSchemaModel
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**externalDocs**  <br>*optional*|[ExternalDocs](#externaldocs)|
|**properties**  <br>*optional*|< string, object > map|
|**reference**  <br>*optional*|string|
|**schema**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|


<a name="stringschemaproperty"></a>
### StringSchemaProperty
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**access**  <br>*optional*|string|
|**allowEmptyValue**  <br>*optional*|boolean|
|**description**  <br>*optional*|string|
|**example**  <br>*optional*|object|
|**format**  <br>*optional*|string|
|**name**  <br>*optional*|string|
|**position**  <br>*optional*|integer (int32)|
|**readOnly**  <br>*optional*|boolean|
|**required**  <br>*optional*|boolean|
|**schema**  <br>*optional*|string|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**vendorExtensions**  <br>*optional*|< string, object > map|
|**xml**  <br>*optional*|[Xml](#xml)|


<a name="tag"></a>
### Tag

|Name|Description|Schema|
|---|---|---|
|**description**  <br>*optional*|A short description for the tag|string|
|**externalDocs**  <br>*optional*||[ExternalDocs](#externaldocs)|
|**name**  <br>*optional*|The name of the tag|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|


<a name="team"></a>
### Team

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*|Team id|string|
|**name**  <br>*optional*|Team name|string|
|**source**  <br>*optional*|The value is to identify whether the team is created from global team assignment or from user or by system|enum (USER, GLOBAL_ASSIGNMENT, SYSTEM)|


<a name="unpublishinfo"></a>
### UnpublishInfo

|Name|Description|Schema|
|---|---|---|
|**apiId**  <br>*optional*|API id for the API to be unpublished. This field is required.|string|
|**serviceRegistryIds**  <br>*optional*|List of ids of the service registries from which the API has to be unpublished. This field is required.|< string > array|


<a name="unpublishresponse"></a>
### UnpublishResponse

|Name|Description|Schema|
|---|---|---|
|**apiId**  <br>*optional*|API id of the API published.|string|
|**apiName**  <br>*optional*|API name of the API published.|string|
|**apiVersion**  <br>*optional*|API version of the API published.|string|
|**serviceRegistryUnpublishResponses**  <br>*optional*|Contains unpublish status of the API for each service registry in the request.|< [ServiceRegistryUnpublishResponse](#serviceregistryunpublishresponse) > array|


<a name="version"></a>
### Version

|Name|Schema|
|---|---|
|**apiId**  <br>*optional*|string|
|**versionNumber**  <br>*optional*|string|


<a name="websocketapi"></a>
### WebsocketAPI
*Polymorphism* : Inheritance  
*Discriminator* : type


|Name|Schema|
|---|---|
|**apiTags**  <br>*optional*|< string > array|
|**description**  <br>*optional*|string|
|**externalDocs**  <br>*optional*|< [ExternalDocs](#externaldocs) > array|
|**messages**  <br>*optional*|< [MessageFrame](#messageframe) > array|
|**nativeUri**  <br>*optional*|< string > array|
|**parameters**  <br>*optional*|< string, [AbstractParameter](#abstractparameter) > map|
|**serviceRegistryDisplayName**  <br>*optional*|string|
|**supportedSubProtocols**  <br>*optional*|< string > array|
|**tags**  <br>*optional*|< [Tag](#tag) > array|
|**title**  <br>*optional*|string|
|**type**  <br>*optional*|string|
|**uri**  <br>*optional*|< string > array|
|**version**  <br>*optional*|string|


<a name="xml"></a>
### Xml

|Name|Description|Schema|
|---|---|---|
|**attribute**  <br>*optional*|Declares whether the property definition translates to an attribute instead of an element. Default value is false|boolean|
|**name**  <br>*optional*|Replaces the name of the element/attribute used for the described schema property. When defined within items, it will affect the name of the individual XML elements within the list. When defined alongside type being array (outside the items), it will affect the wrapping element and only if wrapped is true. If wrapped is false, it will be ignored|string|
|**namespace**  <br>*optional*|The URI of the namespace definition|string|
|**prefix**  <br>*optional*|The prefix to be used for the name|string|
|**vendorExtensions**  <br>*optional*||< string, object > map|
|**wrapped**  <br>*optional*|MAY be used only for an array definition. Signifies whether the array is wrapped (for example, <books><book/><book/></books>) or unwrapped (<book/><book/>). Default value is false|boolean|


<a name="xpath"></a>
### Xpath

|Name|Schema|
|---|---|
|**namespaces**  <br>*optional*|< [Namespaces](#namespaces) > array|
|**xpath**  <br>*optional*|string|




<a name="securityscheme"></a>
## Security

<a name="basic"></a>
### Basic
API Gateway Administrator and API Gateway provider

*Type* : basic



