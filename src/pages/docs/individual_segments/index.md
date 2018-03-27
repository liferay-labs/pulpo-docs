---
title: "Individual Segments"
description: "Individual Segments API."
layout: "guide"
icon: "persons"
weight: 7
---

###### {$page.description}

<article id="1">

## The Individual Segment Model

Individual Segments are aggregations of Individuals. 

These aggregations can be:
* Dynamic: A Variable set of individuals matching a certain condition (filter) belong to the Individual Segment. 
* Static: A fixed set of individuals have been manually associated to the Individual Segment. 
 
The following fields are currently supported as part of an Individual Segment:
* *author*
  * identifier
  * name
* *dateCreated*
* *dateModified*
* *fields* - a Map of [Fields](/docs/fields) corresponding to properties of the Individual Segment
* *filter* - an oData filter that defines, for Individual Segments with `segmentType=DYNAMIC`, which Individuals
   belong to this Individual Segment 
* *filterMetadata* - a placeholder for extra information about the filter
* *identifier*
* *individualCount* - the current count of individuals associated to the Individual Segment
* *interests* - a Map of interest [Fields](/docs/fields)
* *name* - The name of the Individual Segment
* *scope* - The scope of the Individual Segment, whether this segment belongs to a user or is shared between the members of a project. The accepted values are `USER` and `PROJECT`.
* *segmentType* - defines if the Individual Segment aggregates Individuals dynamically or statically. The accepted values are `STATIC` and `DYNAMIC`
* *status* - defines if the Individual Segment accepts memberships or not. The accepted values are `ACTIVE` and `INACTIVE`

</article>

<article id="2">

## Individual Segment Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `individual-segments` pointing to the
collection of Individual Segments.

This API supports [pagination](/docs/general#pagination) and [sorting](/docs/general#sorting).

The response will contain inside the `_embedded` section, a list of individual segments
under the key `individual-segments`.

This is an example of a response to this url: `http://localhost:8084/my-project/individual-segments?page=0&size=20`

```json
{
    "_embedded": {
        "individual-segments":[
          {
             "dateCreated":"2017-11-15T16:23:35Z",
             "dateModified":"2017-11-15T16:23:35Z",
             "filter":null,
             "filterMetadata":null,
             "identifier":"AV_Afi6-Y3UMLZEdmkBE",
             "name":"Friends",
             "segmentType":"STATIC",
             "status":"ACTIVE",
             "author": {
                 "name":"John Doe",
                 "identifier":"12345"
              },
             "interests": {
                "business": [
                    {
                        "ownerIdentifier": "AWJL5ucfj5w_cS02rIwL",
                        "dataSourceIdentifier": "AWI_0f2_q_9uZvuIRBN3",
                        "fieldType": "http://schema.org/Number",
                        "ownerType": "individualSegment",
                        "context": "interests",
                        "name": "business",
                        "value": "1.609438",
                        "dateModified": "2018-03-27T10:46:59+0000",
                        "label": null,
                        "identifier": "AWJnEXGW1sOrFJL935Fw"
                    }
                ],
                "connected experiences": [
                    {
                        "ownerIdentifier": "AWJL5ucfj5w_cS02rIwL",
                        "dataSourceIdentifier": "AWI_0f2_q_9uZvuIRBN3",
                        "fieldType": "http://schema.org/Number",
                        "ownerType": "individualSegment",
                        "context": "interests",
                        "name": "connected experiences",
                        "value": "2.4849067",
                        "dateModified": "2018-03-27T10:46:59+0000",
                        "label": null,
                        "identifier": "AWJnEXG21sOrFJL935Fy"
                    }
                ]
             },
             "fields": {                   
             }, 
             "_links":{
                "self":{
                   "href":"http://localhost:8084/my-project/individual-segments/AV_Afi6-Y3UMLZEdmkBE"
                },
                "individual-segments":{
                   "href":"http://localhost:8084/my-project/individual-segments{?filter}",
                   "templated":true
                },
                "individuals":{
                   "href":"http://localhost:8084/my-project/individual-segments/AV_Afi6-Y3UMLZEdmkBE/individuals{?filter}",
                   "templated":true
                },
                "memberships": {
                    "href": "http://localhost:8084/my-project/individual-segments/AV_81ueo7IU2hIVahEUv/memberships"
                }
             }
          }
        ]
    },
    "_links":{
       "self":{
           "href":"http://localhost:8084/my-project/individual-segments?page=0&size=20"
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

Creation of new Individual Segments is supported making a `POST` to the Collection URL. This is
an example of the body passed to this POST request: 

```json
{
    "name" : "My First IndividualSegment",
    "filter" : "(demographics/age/value eq '30')",
    "segmentType" : "DYNAMIC"
}
```

Navigating through the list of entities, the link to each entity can be found with the rel `self`. 
That same url can be also used for delete (`DELETE` method) and update (`PUT` method). 

</article>

<article id="3">

## Individual Segment Links

As part of the links of each individual, the following links can be found using these keys:
* `individual-segments` - The collection of Individual Segments
* `individuals` - The collection of Individuals who belong to this Individual Segment. This collection can be filtered as explained in [filtering](/docs/general#filtering), and transformations can be applied on it as explained in [transformations](/docs/general#transformations).
* `memberships` - The collection of Memberships of this Individual Segment. This collection can be used to add new members to this individual segment manually, as described in #4.

</article>

<article id="4">

## Individual Segment Membership Collection

Creation of new Individual-Individual Segment memberships is supported only for Individual Segments with `status=ACTIVE`
and `segmentType=STATIC` by making a `POST` to the `memberships` Collection URL of each individual segment . 
This is an example of the body passed to this POST request to the URL 
`http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships` 

```json
{
    "individualIdentifier" : "my-individual-identifier"
}
```

A `DELETE` request to the URL `http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships/my-individual-identifier` removes
an existing Individual-Individual Segment membership.

</article>

<article id="5">

## Individual Segment Membership Count

The current value of the count of Individuals that are members of an Individual Segment can be obtained from 
the `individualCount` field of the `individual-segment` resource.

The historical values of the count of Individuals that are members of an Individual Segment are stored
as [Fields](/docs/fields) and they can be obtained through the [filtering](/docs/general#filtering) options as described
in the [Retrieving historical values](/docs/fields#3) section. 
 
</article>

<article id="6">

## Filtering and Sorting Individual Segments Collection

Individual Segment collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individual Segments with interests for business: `?filter=(interests/business/value gt '0')`
* Individual Segments with interests for software and sports greater than a score of 10: `?filter=(interests/software/value gt '10') and (interests/sports/value gt '10'))`

</article>