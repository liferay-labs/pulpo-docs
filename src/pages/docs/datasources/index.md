---
title: "Data Sources"
description: "Data Sources API"
layout: "guide"
icon: "database"
weight: 2
---

###### {$page.description}

<article id="1">

## The Data Source Model

DataSources support a subset of the Type [DataFeed](http://schema.org/DataFeed) from Schema.org

The following fields are currently supported:
* *about*
* *author*
  * identifier
  * name
* *dateCreated*
* *dateModified*
* *identifier*
* *lastConfigChange* - contains information about the configuration associated to this Data Source.
  * transactionId - identifies the latest configuration operation
  * status - the status of the latest configuration operation. Possible values are:
    * `SENT` - the configuration has been sent by the Data Source, but no confirmation has been received
    * `OK_RECEIVED` - the configuration has been sent by the Data Source, and a confirmation that been received with no errors.
    * `ERROR_RECEIVED` - the configuration has been sent by the Data Source, and a confirmation that been received with errors. 
    Your administrator can use the transactionId to track the error on the log files.
* *name*
* *provider*
  * type - See the [DataSource Provider](#provider) section for more details 
* *url*
* *subjectOf* - An event about this Data Source
  * name
  * startDate
  * endDate
  * location
  * sameAs - the URL of the Event

</article>


<article id="2">

## DataSources Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `data-sources` pointing to the
collection of Data Sources.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of data sources
under the key `data-sources`.

This is an example of a response to this url: `http://localhost:8084/my-project/data-sources?page=0&size=1`

```json
{
    "_embedded": {
        "data-sources": [
            {
                "dateCreated": "2017-09-14T12:00:04Z",
                "dateModified": "2017-09-14T12:00:04Z",
                "identifier": "AV6AQqVHWUV1yhbro9xD",
                "lastConfigChange": {
                    "transactionId": "AWJzfSDgdgFFtKdpvGBL",
                    "status": "SENT"
                },
                "name": "my Liferay 6.2",
                "provider": {
                    "type": "CSV"
                },
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/data-sources/AV6AQqVHWUV1yhbro9xD"
                    },
                    "data-sources": {
                        "href": "http://localhost:8084/my-project/data-sources"
                    }
                }
            }
        ]
    },
    "_links": {
        "first": {
            "href": "http://localhost:8084/my-project/data-sources?page=0&size=1"
        },
        "self": {
            "href": "http://localhost:8084/my-project/data-sources?page=0&size=1"
        },
        "next": {
            "href": "http://localhost:8084/my-project/data-sources?page=1&size=1"
        },
        "last": {
            "href": "http://localhost:8084/my-project/data-sources?page=1&size=1"
        }
    },
    "page": {
        "size": 1,
        "totalElements": 2,
        "totalPages": 2,
        "number": 0
    }
}
```

Creation of new Data Sources is supported making a `POST` to the Collection URL. This is
an example of the body passed to this POST request: 

```json
{
    "name" : "My First DataSource",
    "provider" : {
        "type" : "CSV"
    },
    "author" : {
        "name" : "Shinn",
        "identifier" : "ABC1234"
    }
}
```

Navigating through the list of entities, the link to each entity can be found with the rel `self`. 
That same url can be also used for delete (`DELETE` method) and update (`PUT` method).

</article>

<article id="provider">

## DataSource Provider 

The `provider` field of the DataSource contains the specific configuration for a provider (e.g. Liferay, CSV). 
As this information changes from one type of provider to other, the set of fields is different, too. 

A valid DataSource provider field must contain at least a field `type` with one of the supported types as values.
Currently, the supported values are `LIFERAY` and `CSV`. 

### Liferay DataSource Provider

The following fields are supported for a Liferay Provider:
* *type* - the value must be `LIFERAY` for a Liferay Provider
* *analyticsConfiguration* - Contains the analytics configuration for the DataSource
  * analyticsKey
  * enableAllSites - If the value is `true`, all the sites in the instance will send analytics, ignoring the configuration in the `sites` field.
  * sites - A list elements with the structure:
    * enableAllChildren - If the value is `true`, all the children of this site will send analytics
    * identifier - The primary key of the site
* *contactsConfiguration* - Contains the contacts configuration for the DataSource
  * enableAllContacts - If the value is `true`, all the contacts in the instance will be synchronized, ignoring the configuration in the `organizations` and `userGroups` fields.
  * organizations - A list elements with the structure:
    * enableAllChildren - If the value is `true`, all the contacts in the children of this organization will be synchronized
    * identifier - The primary key of the organization
  * userGroups - A list elements with the structure:
    * enableAllChildren - If the value is `true`, all the contacts in the children of this user group will be synchronized
    * identifier - The primary key of the user group  
* *instanceInfo* - Contacts information about the Liferay Portal instance
  * companyId
  * lcsInstallationId


This is an example of the body passed  to the `POST` request to create a DataSource with a `provider` field of type 
Liferay:

```json
{
    "name" : "Liferay Intranet DataSource",
    "provider" : {
        "analyticsConfiguration" : {
            "analyticsKey" : "My-Key-For-Analytics",
            "enableAllSites" : false,
            "sites" : [
                {
                    "enableAllChildren" : true,
                    "identifier" : "1"
                }
            ]
        },
        "contactsConfiguration" : {
            "enableAllContacts" : false,
            "organizations" : [
                {
                    "enableAllChildren" : true,
                    "identifier" : "2"
                },
                {
                    "enableAllChildren" : false,
                    "identifier" : "3"
                }
            ],
            "userGroups" : [
                {
                    "enableAllChildren" : false,
                    "identifier" : "4"
                }
            ]
        },
        "instanceInfo" : {
            "companyId" : "1",
            "lcsInstallationId" : "1"
        },
        "type" : "LIFERAY"
    },
    "author" : {
        "name" : "Shinn",
        "identifier" : "ABC1234"
    }
}
``` 

</article>