---
title: "Account Segments"
description: "Account Segments API."
layout: "guide"
icon: "person-card"
weight: 8
---

###### {$page.description}

<article id="1">

## The Account Segment Model

Account Segments are aggregations of Accounts. 

These aggregations can be:
* Dynamic: A Variable set of accounts matching a certain condition (filter) belong to the Account Segment. 
* Static: A fixed set of accounts have been manually associated to the Account Segment. 
 
The following fields are currently supported as part of an Account Segment:
* *dateCreated*
* *dateModified*
* *filter* - an oData filter that defines, for Account Segments with `segmentType=DYNAMIC`, which Accounts
   belong to this Account Segment 
* *identifier*
* *name* - The name of the Account Segment
* *segmentType* - defines if the Account Segment aggregates Accounts dynamically or statically. The accepted values are `Type.STATIC` and `Type.DYNAMIC`

</article>

<article id="2">

## Account Segment Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `account-segments` pointing to the
collection of Account Segments.

This API supports [pagination](/docs/general#pagination) and [sorting](/docs/general#sorting).

The response will contain inside the `_embedded` section, a list of account segments
under the key `account-segments`.

This is an example of a response to this url: `http://localhost:8084/my-project/account-segments?page=0&size=20`

```json
{
    "_embedded": {
        "account-segments":[
            {
                 "dateCreated":"2017-11-15T16:23:35Z",
                 "dateModified":"2017-11-15T16:23:35Z",
                 "filter":null,
                 "identifier":"AV_Afi6-Y3UMLZEdmkBE",
                 "name":"Partners",
                 "segmentType":"STATIC",
                 "_links":{
                    "self":{
                       "href":"http://localhost:8084/my-project/account-segments/AV_Afi6-Y3UMLZEdmkBE"
                    },
                    "account-segments":{
                       "href":"http://localhost:8084/my-project/account-segments{?filter}",
                       "templated":true
                    },
                    "accounts":{
                       "href":"http://localhost:8084/my-project/account-segments/AV_Afi6-Y3UMLZEdmkBE/accounts{?filter}",
                       "templated":true
                    }
                 }
            }
           ]
       },
    "_links":{
       "self":{
           "href":"http://localhost:8084/my-project/account-segments?page=0&size=20"
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

Creation of new Account Segments is supported making a `POST` to the Collection URL. This is
an example of the body passed to this POST request: 

```json
{
    "name" : "My First AccountSegment",
    "filter" : "(organization/employees/value eq '30')",
    "segmentType" : "DYNAMIC"
}
```

Navigating through the list of entities, the link to each entity can be found with the rel `self`. 
That same url can be also used for delete (`DELETE` method) and update (`PUT` method). 

</article>

<article id="3">

## Account Segment Links

As part of the links of each account, the following links can be found using these keys:
* `account-segments` - The collection of Account Segments
* `accounts` - The collection of Accounts who belong to this Account Segment. This collection can be filtered as explained in [filtering](/docs/general#filtering).

</article>

<article id="4">

## Account Segment Membership Collection

Creation of new Account-Account Segment memberships is supported only for Account Segments with `segmentType=STATIC` by making 
a `POST` to the `memberships` Collection URL of each account segment . This is an example of the body passed to this POST request to the URL 
`http://localhost:8084/my-project/account-segments/my-account-segment-identifier/memberships` 

```json
{
    "accountSegmentIdentifier" : "my-account-segment-identifier",
    "accountIdentifier" : "my-account-identifier"
}
```

A `DELETE` request to the URL `http://localhost:8084/my-project/account-segments/my-account-segment-identifier/memberships/my-account-identifier` removes
an existing Account-Account Segment membership. 

</article>