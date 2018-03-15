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

Activities can aggregate subactivities inside to provide a higher level of detail. E.g. An activity could be a visit
to a website and it could have subactivities such as "View the Home Page", "Submit a Form", "Click on a button"... etc

The entity field contains the following fields:
* *identifier*
* *startTime*
* *endTime*
* *name*
* *description*
* *activityType*
* *ownerType* - The entity that performed this activity (e.g. an Individual, an Account...)
* *ownerIdentifier* - The Identifier of the entity that performed this activity.
* *subactivities* - A list with the more detailed activities that compose this activity.

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
                "description": "3 Documents, 2 Pages, 1 Form",
                "ownerIdentifier": "AWIqHXWWV1ufgGLxavLQ",
                "ownerType": "individual",
                "activityType": "Web",
                "startTime": "2018-03-15T14:43:16Z",
                "endTime": "2018-03-15T14:43:16Z",
                "subactivities": [
                    {
                        "name": "Visit Home Page",
                        "description": "Spent 5 min reading",
                        "createdDate": "2018-03-15T14:43:16Z",
                        "subactivityType": "View"
                    },
                    {
                        "name": "Download Document: Use Cases",
                        "createdDate": "2018-03-15T14:43:16Z",
                        "subactivityType": "Download"
                    }
                ],
                "identifier": "AWIqHXbVV1ufgGLxavLg",
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/activities/AWIqHXbVV1ufgGLxavLg"
                    },
                    "individual": {
                        "href": "http://localhost:8084/my-project/individuals/AWIqHXWWV1ufgGLxavLQ"
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