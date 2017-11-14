---
title: "Data Sources"
description: "Data Sources API"
layout: "guide"
icon: "database"
weight: 3
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
* *name*
* *provider*
  * name
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

Creation of new Data Sources is supported making a `POST` to the Collection URL. This is
an example of the body passed to this POST request: 

```json
{
	"name" : "My First DataSource",
	"projectId" : "my-project",
	"provider" : {
		"name" : "liferay-de"
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