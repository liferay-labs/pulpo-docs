---
title: "Individual Segments"
description: "Individual Segments API."
layout: "guide"
icon: "persons"
weight: 6
---

###### {$page.description}

<article id="1">

## The Individual Segment Model

Individual Segments are aggregations of Individuals. 

These aggregations can be:
* Dynamic: A Variable set of individuals matching a certain condition (filter) belong to the Individual Segment. 
* Static: A fixed set of individuals have been manually associated to the Individual Segment. 
 
The following fields are currently supported as part of an Individual Segment:
* *dateCreated*
* *dateModified*
* *filter* - an oData filter that defines, for Individual Segments with `segmentType=DYNAMIC`, which Individuals
   belong to this Individual Segment 
* *identifier*
* *name* - The name of the Individual Segment
* *segmentType* - defines if the Individual Segment aggregates Individuals dynamically or statically. The accepted values are `Type.STATIC` and `Type.DYNAMIC`

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
             "identifier":"AV_Afi6-Y3UMLZEdmkBE",
             "name":"Friends",
             "segmentType":"STATIC",
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
* `individuals` - The collection of Individuals who belong to this Individual Segment. This collection can be filtered as explained in [filtering](/docs/general#filtering).

</article>

<article id="4">

## Individual Segment Membership Collection

Creation of new Individual-Individual Segment memberships is supported only for Individual Segments with `segmentType=STATIC` by making 
a `POST` to the `memberships` Collection URL of each individual segment . This is an example of the body passed to this POST request to the URL 
`http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships` 

```json
{
    "individualSegmentIdentifier" : "my-individual-segment-identifier",
    "individualIdentifier" : "my-individual-identifier"
}
```

A `DELETE` request to the URL `http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships/my-individual-identifier` removes
an existing Individual-Individual Segment membership. 

</article>