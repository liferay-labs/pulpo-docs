---
title: "Data Sources"
description: "Data Sources API"
layout: "guide"
icon: "database"
weight: 1
---

###### {$page.description}

<article id="1">

## Navigation to obtain URLs

URLs are not part of this API, they may change at any moment. URLs must be asked to
the service before making any request.

The URLs can be obtained making a request to the root resource of the Service "/".

The response in json HAL format will contain a "_links" object with the different links to be used. For example: 

```json
{
    "name": "pulpo-api",
    "description": "API for consuming PULPO Services",
    "_links": {
        "self": {
            "href": "http://localhost:8084/"
        },
        "data-sources": {
            "href": "http://localhost:8084/{projectId}/data-sources",
            "templated": true
        }
    }
}
```

The URL for managing data sources has the key "data-sources" inside the section "_links". 
This URL can be used to obtain the entities (GET method) and create new ones (POST method).

It can be paginated and sorted using the params (page, size and sort).
The projectId variable must be replaced with the projectId of the current project.

Navigating through the list of entities, the link to each entity can be found with the rel "self". 
That same url can be also used for delete (DELETE method) and update (PUT method).

This is an example of a response to this url: http://localhost:8084/my-project/data-sources?page=0&size=1

```json
{
    "_embedded": {
        "data-sources": [
            {
                "dateCreated": "2017-09-14T12:00:04Z",
                "dateModified": "2017-09-14T12:00:04Z",
                "identifier": "AV6AQqVHWUV1yhbro9xD",
                "name": "my Liferay 6.2",
                "provider": {
                    "name": "liferay-de"
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
From this response, you can obtain the total number of existing elements under the "page" block and also
the links to other pages of data sources.

</article>

<article id="2">

## The Data Source Model

DataSources support a subset of the Type [DataFeed] from Schema.org

The following fields are currently supported:
* author
* dataCreated
* dateModified
* identifier
* name
* provider
* url
* subjectOf

</article>