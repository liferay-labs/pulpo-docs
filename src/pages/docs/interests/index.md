---
title: "Interests"
description: "Interests API"
layout: "guide"
icon: "circle-arrow"
weight: 12
---

###### {$page.description}

<article id="1">

## The Interest Model

Interests contain information about diffferent topics that other entities ([individuals](/docs/individuals), individual segments... etc) are interested on .

Each Interest represent certain interest of an entity about a topic for a certain day.
Interests are calculated using a 30 day window range, therefore, even when there
is a score calculated for every day, that score takes into account the previuos
30 days.

The interest field contains the following fields:
* *identifier*
* *dateRecorded* - The date this score was calculated
* *name* - The name of the topic
* *score* - The score for this topic (the higher the most interested on this topic)
* *ownerType* - The entity that owns this field (e.g. an Individual, an Account...)
* *ownerIdentifier* - The Identifier of the entity that owns this field.
* *pagesVisited* - A list of pageVisit that justifies this interest. Each pageVisit has the following fields:
  * url - url of the page
  * title - title of the page
  * description - description of the page
  * uniqueVisitsCount - number of visits to this page

</article>


<article id="2">

## Interests Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `interests` pointing to the
collection of Interests.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of interests
under the key `interests`.

This is an example of a response to this url: http://localhost:8084/my-project/interests?page=0&size=1

```json
{
    "_embedded": {
        "interests": [
            {
     			"dateRecorded": "2018-03-05T00:00:00+0000",
                "identifier": "AWMYbBfSgcT3bCtqgwde",
                "name": "intranets",
                "score": 1.0986122886681098,
                "ownerIdentifier": "AV-0-dcI4MMBozrmZ0UM",
                "ownerType": "individual",
                "pagesVisited": [
					{
						"title": "Products for Liferay",
						"uniqueVisitsCount": 1,
						"description": "Liferay DXP, Liferay De, Sync...",
						"url": "https://www.liferay.com/products"
					},
					{
						"title": "Liferay Digital Experience Platform",
						"uniqueVisitsCount": 5,
						"description": "Portals, Intranets, Platforms...",
						"url": "https://www.liferay.com/en/home"
					}
				],
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/interests/AWMYbBfSgcT3bCtqgwde"
                    },
                    "individual": {
                        "href": "http://localhost:8084/my-project/individuals/AV-0-dcI4MMBozrmZ0UM"
                    },
                    "interests": {
                        "href": "http://localhost:8084/my-project/interests{?filter}",
                        "templated": true
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/interests?page=0&size=1"
        }
    },
    "page": {
        "size": 20,
        "totalElements": 1,
        "totalPages": 1,
        "number": 0
    }
}
```
                      
Creation of new Interests or Update of Interests manually is not supported. Interests are automatically
generated and updated from the Interst Chunks sent by the algorithms used to calculate this
based on the Analytics data.

Deletion of existing Interests is not allowed for now either. 

Navigating through the list of interests, the link to each interest can be found with the rel `self`, and also
a link to the entity owning it with the rel of the entity (e.g. individual or individual-segment). 

</article>

<article id="3">

## Retrieving historical values for a specific topic of interest

Obtaining the historical value of an interest can be done using using 
the [filtering](/docs/general#filtering) option in the Interest Collection. The `ownerType` and `ownerIdentifier` 
can be used to idenfity the entity interested and the name to obtain interests for just
one topic.
The [sorting](/docs/general#sorting) option can be used to obtain the interests by date for example.

These are some examples of Interests filtering to retrieve historical values of certain Individual and Individual Segment properties:

* The historical values of the interst on "portals" for an Individual: `((name eq 'portals') and (ownerType eq 'individual') and (ownerIdentifier eq 'the-individual-identifier'))`
* The historical values of the topic 'Business' for an Individual Segment: `((name eq 'business') and (ownerType eq 'individual-segment') and (ownerIdentifier eq 'the-individual-segment-identifier'))` 
* The historical values of the interst on "intrantes" with a score higher than 10 for any Individual: `((score gt '10') and (name eq 'intranets') and (ownerType eq 'individual'))`

</article>

<article id="4">

## Transformations on Interest Collection

Transformations can be applied on Interests collection as explained in [transformations](/docs/general#transformations).

The only transformation allowed for the Interest Collection is `groupby` by `day` or `month`.

These are some examples of transformations:

* Interest group by day of creation: `?apply=compute(day(dateRecorded) as day)/groupby((day))`

This is an example of a response to this url: ` http://localhost:8084/my-project/interests?apply=compute(day(dateRecorded) as day)/groupby((day))`

```json
{
    "_embedded": {
        "interest-transformations": [
            {
                "count": 0,
                "intervalInitDate": "2018-05-03T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-04T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-05T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-06T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-07T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-08T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-09T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-10T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-11T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-12T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-13T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-14T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-15T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-16T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-17T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-18T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-19T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-05-20T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 7,
                "intervalInitDate": "2018-05-21T00:00:00Z",
                "scoreAvg": 10.3,
                "viewSum": 0
            },
            {
                "count": 4,
                "intervalInitDate": "2018-05-22T00:00:00Z",
                "scoreAvg": 41.86,
                "viewSum": 0
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/1527005536349/interests?apply=compute%28day%28dateRecorded%29%20as%20day%29%2Fgroupby%28%28day%29%29&page=0&size=20"
        }
    },
    "page": {
        "size": 20,
        "totalElements": 20,
        "totalPages": 1,
        "number": 0
    }
}

```

* Interest group by month of creation: `?apply=compute(month(dateRecorded) as month)/groupby((month))`

This is an example of a response to this url: ` http://localhost:8084/my-project/interests?apply=compute(month(dateRecorded) as month)/groupby((month))`

```json
{
    "_embedded": {
        "interest-transformations": [
            {
                "count": 0,
                "intervalInitDate": "2016-10-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2016-11-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2016-12-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-01-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-02-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-03-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-04-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-05-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-06-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-07-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-08-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-09-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-10-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-11-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2017-12-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-01-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-02-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-03-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 0,
                "intervalInitDate": "2018-04-01T00:00:00Z",
                "scoreAvg": 0.0,
                "viewSum": 0
            },
            {
                "count": 4,
                "intervalInitDate": "2018-05-01T00:00:00Z",
                "scoreAvg": 41.86,
                "viewSum": 0
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/1527005923917/interests?apply=compute%28month%28dateRecorded%29%20as%20month%29%2Fgroupby%28%28month%29%29&page=0&size=20"
        }
    },
    "page": {
        "size": 20,
        "totalElements": 20,
        "totalPages": 1,
        "number": 0
    }
}

```


</article>