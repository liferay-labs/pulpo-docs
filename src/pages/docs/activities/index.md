---
title: "Activities"
description: "Activities API"
layout: "guide"
icon: "hammer"
weight: 10
---

###### {$page.description}

<article id="1">

## The Activity Model

Activities contain information about behaviour and actions performed by different entities ([individuals](/docs/individuals), accounts, individual segments... etc).

Activities can aggregate actions inside to provide a higher level of detail. E.g. An activity could be a visit
to a website and it could have actions such as "View the Home Page", "Submit a Form", "Click on a button"... etc

The entity activity contains the following fields:
* *identifier*
* *startTime*
* *endTime*
* *name*
* *description*
* *activityType*
* *ownerType* - The entity that performed this activity (e.g. an Individual, an Account...)
* *ownerIdentifier* - The Identifier of the entity that performed this activity.
* *actions* - A list with of the actions that compose this activity.

The entity action contains the following fields:
* *startTime*
* *name*
* *description*
* *actionType*
* *object* - The object upon which the action is carried out

The entity object contains the following fields:
* *identifier*
* *name*
* *description*
* *objectType*
* *url*

</article>


<article id="2">

## Activities Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `activities` pointing to the
collection of Activities.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of activities
under the key `activities`.

This is an example of a response to this url: http://localhost:8084/my-project/activities?page=0&size=1

```json
{
    "_embedded": {
        "activities": [
            {
				"name": "Visit liferay.com",
				"actions": [
					{
						"name": "Visit Home Page",
						"description": "Spent 5 min reading",
						"startTime": "2018-03-06T03:50:43Z",
						"actionType": "Download",
						"object": {
							"name": "Home Page",
							"objectType": "Page",
							"url": "http://homepage.com",
							"description": "Description of the page",
							"identifier": "194EF"
						}
					},
					{
						"name": "Download Document",
						"description": "Spent 5 min reading",
						"startTime": "2018-03-16T10:47:56Z",
						"actionType": "Download",
						"object": {
							"name": "Use Cases 2018",
							"objectType": "PDF",
							"url": "http://document-download.com",
							"description": "A document with a long description",
							"identifier": "12345-ABCDEF"
						}
					}
				],
				"description": "3 Documents, 2 Visits",
				"ownerIdentifier": "AWIubFositjEuNSqYpR",
				"ownerType": "individual",
				"activityType": "Web",
				"startTime": "2018-03-06T03:50:43Z",
				"endTime": "2018-03-06T03:50:43Z",
				"identifier": "AWIubHEhitjEuNSqYph",
				"_links": {
					"self": {
						"href": "http://localhost:8084/my-project/activities/AWIubHEhitjEuNSqYph"
					},
					"individual": {
						"href": "http://localhost:8084/my-project/individuals/AWIubFositjEuNSqYpR"
					},
					"activities": {
						"href": "http://localhost:8084/my-project/activities?page=0&size=20{&filter,sort*}"
					}
				}
			}
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/activities?page=0&size=20"
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

Creation of new Activities or Update of Activities manually is not supported. Activities are automatically
generated and updated from the Activity Chunks sent by the different Connectors.

Deletion of existing Activities is not allowed for now either. 

Navigating through the list of activities, the link to each activity can be found with the rel `self`. 

</article>