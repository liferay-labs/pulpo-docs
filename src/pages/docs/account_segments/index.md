---
title: "Account Segments"
description: "Account Segments API."
layout: "guide"
icon: "person-card"
weight: 9
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
                    },
                    "memberships": {
                        "href":"http://localhost:8084/my-project/account-segments/AV_81uji7IU2hIVahEU6/memberships"
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
* `memberships` - The collection of Memberships of this Account Segment. This collection can be used to add new members to this account segment manually, as described in #4.

</article>

<article id="4">

## Account Segment Membership Collection

Creation of new Account-Account Segment memberships is supported only for Account Segments with `segmentType=STATIC` by making 
a `POST` to the `memberships` Collection URL of each account segment . This is an example of the body passed to this POST request to the URL 
`http://localhost:8084/my-project/account-segments/my-account-segment-identifier/memberships` 

```json
{
    "accountIdentifier" : "my-account-identifier"
}
```

A `DELETE` request to the URL `http://localhost:8084/my-project/account-segments/my-account-segment-identifier/memberships/my-account-identifier` removes
an existing Account-Account Segment membership. 

</article>

<article id="5">

## Account Segment Membership Count

The current value of the count of Accounts that are members of an Account Segment can be obtained from 
the `totalElements` field of the `accounts` collection.

The historical values of the count of Accounts that are members of an Account Segment are stored 
as [Fields](/docs/fields) with the name `accountCount` and associated to the Individual Segment through 
the `ownerType` and `ownerIdentifier` properties. For example, using the oData filter
`(name eq 'accountCount') and (ownerype eq 'account-segment') and (ownerIdentifier eq 'AV_Afi6-Y3UMLZEdmkBE')`
returns a collection of fields with the historical count values for the Account Segment with 
identifier `AV_Afi6-Y3UMLZEdmkBE`.

The latest value of the count is also stored in the fields of the Account Segment and therefore it
can be used to filter and sort the collection of Account Segments. However, it is 
very important to know that *this value may be outdated* since this is just the latest
historical value that is updated once a day. For the accurate number of members, the
totalElements field from the collection should be retrieved.
 
For example, this URL would obtain the
the collection of Account segments sorted by number of members.
`http://localhost:8084/my-project/account-segments?sort=fields/accountCount/value` 
 

</article>