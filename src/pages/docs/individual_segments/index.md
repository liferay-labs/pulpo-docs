---
title: "Individual Segments"
description: "Individual Segments API."
layout: "guide"
icon: "persons"
weight: 7
---

###### {$page.description}

<article id="individual-segment-model">

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
* *filter* - an oData filter that defines, for Individual Segments with `segmentType=DYNAMIC`, which Individuals
   belong to this Individual Segment 
* *filterMetadata* - a placeholder for extra information about the filter
* *identifier*
* *individualCount* - the current count of individuals associated to the Individual Segment
* *name* - The name of the Individual Segment
* *scope* - The scope of the Individual Segment, whether this segment belongs to a user or is shared between the members of a project. The accepted values are `USER` and `PROJECT`.
* *segmentType* - defines if the Individual Segment aggregates Individuals dynamically or statically. The accepted values are `STATIC` and `DYNAMIC`
* *status* - defines if the Individual Segment accepts memberships or not. The accepted values are `ACTIVE` and `INACTIVE`

</article>

<article id="individual-segment-colleciton">

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
                    "href": "http://localhost:8084/my-project/individual-segments/AV_81ueo7IU2hIVahEUv/memberships{?filter,page,size,sort*}"
                },
                "membership-changes": {
                    "href": "http://localhost:8084/my-project/individual-segments/AV_81ueo7IU2hIVahEUv/membership-changes{?filter,page,size,sort*}"
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

<article id="individual-segment-links">

## Individual Segment Links

As part of the links of each individual segment, the following links can be found using these keys:
* `individual-segments` - The collection of Individual Segments
* `individuals` - The collection of Individuals who belong to this Individual Segment. This collection can be filtered as explained in [filtering](/docs/general#filtering), and transformations can be applied on it as explained in [transformations](/docs/general#transformations).
* `memberships` - The collection of Memberships of this Individual Segment. This collection can be used to add new members to this individual segment manually, as described in [Individual Segment Membership Collection](#individual-segment-membership-collection).
* `membership-changes` - The collection of Membership Changes of this Individual Segment. This collection can be used to track changes in the memberships, as described in [Individual Segment Membership Change Collection](#individual-segment-membership-change-collection).

</article>

<article id="filtering-and-sorting">

## Filtering and Sorting Individual Segments Collection

Individual Segment collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individual Segments that belong to a user with identifier 123456: `?filter=((scope eq 'USER') and (author/identifier eq '123456'')`
* Individual Segments modified after a certain date: `?filter=(datemodified gt 2018-02-13T12:33:12Z)`

</article>

<article id="individual-segment-membership-model">

## The Individual Segment Membership Model

Individual Segment Memberships represent associations of Individuals to Individual Segments. 
 
The following fields are currently supported as part of an Individual Segment:
* *individualSegmentIdentifier* - The identifier of the Individual Segment
* *individualIdentifier* - The identifier of the Individual 
* *status* - The status of the membership. Its value can be `ACTIVE` if the individual is still a member, or `INACTIVE` if the individual is no longer a member. 
* *dateCreated* - The date when the membership was established, in ISO8601 format
* *dateRemoved* - The date when the the individual stopped being a member (membership was deactivated), in ISO8601 format

</article>

<article id="individual-segment-membership-collection">

## Individual Segment Membership Collection

As described in [Individual Segment Links](#individual-segment-links), the `_links` section of the `individual-segment` resource will contain a template link labelled as `memberships` pointing to the
collection of Memberships of this Individual Segment.

This API supports [pagination](/docs/general#pagination) and [sorting](/docs/general#sorting).

The response will contain inside the `_embedded` section, a list of individual segment memberships
under the key `memberships`.

This is an example of a response of a `GET` request to this url: `http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships?page=0&size=20`

```json
{
    "_embedded": {
        "memberships": [
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships/AWONINaxjx1K64tMzvV6"
                    },
                    "individual": {
                        "href": "http://localhost:8084/my-project/individuals/AWONINaxjx1K64tMzvV6"
                    },
                    "individual-segment": {
                        "href": "http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier"
                    }
                },
                "individualSegmentIdentifier": "my-individual-segment-identifier",
                "status": "ACTIVE",
                "dateCreated": "2018-05-23T13:12:10Z",
                "individualIdentifier": "AWONINaxjx1K64tMzvV6"
            },
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships/AWONINdEjx1K64tMzvWB"
                    },
                    "individual": {
                        "href": "http://localhost:8084/my-project/individuals/AWONINdEjx1K64tMzvWB"
                    },
                    "individual-segment": {
                        "href": "http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier"
                    }
                },
                "individualSegmentIdentifier": "my-individual-segment-identifier",
                "status": "INACTIVE",
                "dateRemoved": "2018-05-23T13:12:10Z",
                "dateCreated": "2018-05-23T13:12:10Z",
                "individualIdentifier": "AWONINdEjx1K64tMzvWB"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships?page=0&size=20"
        }
    },
    "page": {
        "size": 20,
        "totalElements": 2,
        "totalPages": 1,
        "number": 0
    }
}
```

Creation of new Individual Segment Memberships is supported only for Individual Segments with `status=ACTIVE`
and `segmentType=STATIC` by making a `POST` to the `memberships` Collection URL of each individual segment . 
This is an example of the body passed to this POST request to the URL 
`http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships` 

```json
{
    "individualIdentifier" : "my-individual-identifier"
}
```

A `DELETE` request to the URL `http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/memberships/my-individual-identifier` deactivates
an existing Individual Segment Membership.

### Filtering the Individual Segment Membership Collection

Individual Segment Membership collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individual Segment Memberships with active status created before a given date: `?filter=(status eq 'ACTIVE') and (dateCreated lt 2013-05-29T09:13:28Z)`
* Individual Segment Memberships with inactive status removed between two given dates: `?filter=(status eq 'INACTIVE') and (dateCreated gt 2012-05-29T09:13:28Z) and (dateCreated lt 2015-05-29T09:13:28Z)`

</article>

<article id="individual-segment-membership-count">

## Individual Segment Membership Count

The current value of the count of Individuals that are members of an Individual Segment can be obtained from 
the `individualCount` field of the `individual-segment` resource.

The historical values of the count of Individuals that are members of an Individual Segment are stored
as [Fields](/docs/fields) and they can be obtained through the [filtering](/docs/general#filtering) options as described
in the [Retrieving historical values](/docs/fields#individual-segment-links) section. 
 
</article>

<article id="individual-segment-membership-change-model">

## The Individual Segment Membership Change Model

Individual Segment Memberships Changes represent a log with the changes in the memberships of an Individual for an Individual Segment.
 
The following fields are currently supported as part of an Individual Segment:
* *individualSegmentIdentifier* - The identifier of the Individual Segment
* *individualIdentifier* - The identifier of the Individual 
* *individualName* - The name of the Individual 
* *individualEmail* - The email of the Individual 
* *individualsCount* - The number of Individuals who are members at this point in time 
* *operation* - The status of the membership. Its value can be `ADDED` if the membership was created, or `REMOVED` if the membership was deleted. 
* *dateChanged* - The date when the membership was changed, in ISO8601 format
* *dateFirst* - The date when the membership was created, in ISO8601 format

</article>

<article id="individual-segment-membership-change-collection">

## Individual Segment Membership Change Collection

As described in [Individual Segment Links](#individual-segment-links), the `_links` section of the `individual-segment` resource will contain a template link labelled as `membership-changes` pointing to the
collection of Memberships Changes of this Individual Segment. (There is also a shortcut in the root endpoint)

This API supports [filtering](/docs/general#filtering), [pagination](/docs/general#pagination) and [sorting](/docs/general#sorting).

The response will contain inside the `_embedded` section, a list of individual segment memberships
under the key `membership-changes`.

This is an example of a response of a `GET` request to this url: `http://localhost:8084/my-project/individual-segments/my-individual-segment-identifier/membership-changes?page=0&size=20`

```json
{
    "_embedded": {
        "membership-changes": [
            {
                "_links": {
                    "individual": {
                        "href": "http://localhost:8084/my-project-id/individuals/AWOWMtxI7k1UGopTvFDB"
                    },
                    "individual-segment": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier"
                    },
                    "membership": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier/memberships/AWOWMtxI7k1UGopTvFDB"
                    }
                },
                "operation": "REMOVED",
                "individualIdentifier": "AWOWMtxI7k1UGopTvFDB",
                "dateChanged": "2018-05-25T07:28:26Z",
                "dateFirst": "2018-05-25T07:28:26Z",
                "individualSegmentIdentifier": "my-individual-segment-identifier"
            },
            {
                "_links": {
                    "individual": {
                        "href": "http://localhost:8084/my-project-id/individuals/AWOWMtxI7k1UGopTvFDB"
                    },
                    "individual-segment": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier"
                    },
                    "membership": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier/memberships/AWOWMtxI7k1UGopTvFDB"
                    }
                },
                "operation": "ADDED",
                "individualIdentifier": "AWOWMtxI7k1UGopTvFDB",
                "dateChanged": "2018-05-25T07:28:26Z",
                "dateFirst": "2018-05-25T07:28:26Z",
                "individualSegmentIdentifier": "my-individual-segment-identifier"
            },
            {
                "_links": {
                    "individual": {
                        "href": "http://localhost:8084/my-project-id/individuals/AWOWMtks7k1UGopTvFC5"
                    },
                    "individual-segment": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier"
                    },
                    "membership": {
                        "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier/memberships/AWOWMtks7k1UGopTvFC5"
                    }
                },
                "operation": "ADDED",
                "individualIdentifier": "AWOWMtks7k1UGopTvFC5",
                "dateChanged": "2018-05-25T07:28:26Z",
                "dateFirst": "2018-05-25T07:28:26Z",
                "individualSegmentIdentifier": "my-individual-segment-identifier"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project-id/individual-segments/my-individual-segment-identifier/membership-changes?page=0&size=20"
        }
    },
    "page": {
        "size": 20,
        "totalElements": 3,
        "totalPages": 1,
        "number": 0
    }
}
```

### Filtering the Individual Segment Membership Change Collection

Individual Segment Membership Change collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individual Segment Membership Changes that changed with operation `ADDED` before a given date: `?filter=(operation eq 'ADDED') and (dateChanged lt 2013-05-29T09:13:28Z)`
* Individual Segment Memberships Changes that changed with operation `REMOVED` between two given dates: `?filter=(status eq 'REMOVED') and (dateChanged gt 2012-05-29T09:13:28Z) and (dateChanged lt 2015-05-29T09:13:28Z)`

</article>