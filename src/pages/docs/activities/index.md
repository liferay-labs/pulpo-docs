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

The entity activity contains the following fields:
* *identifier*
* *day*
* *startTime*
* *endTime*
* *groupName*
* *description*
* *activityType*
* *applicationId*
* *eventId*
* *object*
* *ownerType* - The entity that performed this activity (e.g. an Individual, an Account...)
* *ownerIdentifier* - The Identifier of the entity that performed this activity.

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

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting), [transformations](/docs/general#transformations), and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of activities
under the key `activities`.

This is an example of a response to this url: http://localhost:8084/my-project/activities?page=0&size=1

```json
{
    "_embedded": {
        "activities": [
            {
				"groupName": "www.liferay.com",
				"description": "Submit Form",
				"ownerIdentifier": "AWIubFositjEuNSqYpR",
				"ownerType": "individual",
				"activityType": "Activity",
				"day": "2018-03-06T00:00:00Z",
				"startTime": "2018-03-06T03:50:43Z",
				"endTime": "2018-03-06T03:50:43Z",
				"identifier": "AWIubHEhitjEuNSqYph",
				"applictionId": "Form",
				"eventId": "formSubmited",
				"eventProperties": {
					"formId": "32cf039a-7a47-4461-82c5-e694d9f29057"
				},
				"_links": {
					"self": {
						"href": "http://localhost:8084/my-project/activities/AWIubHEhitjEuNSqYph"
					},
					"individual": {
						"href": "http://localhost:8084/my-project/individuals/AWIubFositjEuNSqYpR"
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

<article id="activity-transformations">

## Transformations on Activities Collection

Transformations can be applied on Activities collection as explained in [transformations](/docs/general#transformations).

The only transformation allowed for the Activities Collection is `groupby` by `day`, `week` or `month`.

The number of intervals returned when `groupby` is used is determined by the page size. Only the page `0` can be requested.

The returned object has the following fields:

* *totalElements* - The number of elements inside this interval
* *intervalInitDate* - The initial day of this interval 

These are some examples of transformations:

* Activities grouped by day of creation: `?apply=compute(day(dateRecorded) as day)/groupby((day))&page=0&size=3`

This is an example of a response to this url: ` http://localhost:8084/my-project/activities?apply=compute(day(day) as computedDay)/groupby((computedDay))&page=0&size=3`

```json
{
    "_embedded": {
        "activity-transformations": [
            {
                "totalElements": 0,
                "intervalInitDate": "2018-05-20T00:00:00Z"
            },
            {
                "totalElements": 7,
                "intervalInitDate": "2018-05-21T00:00:00Z"
            },
            {
                "totalElements": 4,
                "intervalInitDate": "2018-05-22T00:00:00Z"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/activities?apply=compute%28day%28day%29%20as%20computedDay%29%2Fgroupby%28%28computedDay%29%29&page=0&size=3"
        }
    },
    "page": {
        "size": 3,
        "totalElements": 3,
        "totalPages": 1,
        "number": 0
    }
}

```

* Activities grouped by week of creation: `?apply=compute(week(day) as week)/groupby((week))&page=0&size=3`

This is an example of a response to this url: ` http://localhost:8084/my-project/activities?apply=compute(week(dateRecorded) as week)/groupby((week))&page=0&size=3`

```json
{
    "_embedded": {
        "activity-transformations": [
            {
                "totalElements": 0,
                "intervalInitDate": "2018-06-04T00:00:00Z"
            },
            {
                "totalElements": 0,
                "intervalInitDate": "2018-06-11T00:00:00Z"
            },
            {
                "totalElements": 4,
                "intervalInitDate": "2018-06-18T00:00:00Z"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/activities?apply=compute%28week%28day%29%20as%20week%29%2Fgroupby%28%28week%29%29&page=0&size=3"
        }
    },
    "page": {
        "size": 3,
        "totalElements": 3,
        "totalPages": 1,
        "number": 0
    }
}

```


</article>