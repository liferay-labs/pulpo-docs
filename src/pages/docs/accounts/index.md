---
title: "Accounts"
description: "Accounts API."
layout: "guide"
icon: "people"
weight: 7
---

###### {$page.description}

<article id="1">

## The Accounts Model

Accounts are a complex entity that contain several contexts for each Organization
stored in our system. 

Those contexts are:
* Organization
* Sentiment Analysis
* Sales Interaction
* Transactions
* Firmographic
* Support Interaction
* etc
 
Only some of those contexts are available for now. Each context will contain
certain [Fields](/docs/fields) with information. These fields will correspond to [schema.org](http://schema.org/) Properties

The following fields are currently supported as part of an Invididual:
* *dataSourceIdentifiers* - a map with the different datasource identifiers and the
   datasource individual identifier (the primary key of this account in
   the original datasource) 
* *dateCreated*
* *dateModified*
* *identifier*
* *organization* - a Map of organizational [Fields](/docs/fields) corresponding to properties of the Type [Organization](http://schema.org/Organization) from Schema.org

</article>


<article id="2">

## Accounts Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `accounts` pointing to the
collection of Accounts.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of individuals
under the key `accounts`.

This is an example of a response to this url: `http://localhost:8084/my-project/accounts?page=0&size=1`

```json
{
    "_embedded": {
        "accounts": [
            {
                "dateCreated": "2017-11-14T15:32:06Z",
                "dateModified": "2017-11-14T15:32:07Z",
                "identifier": "AV-7KK2z2uFXwMzLKdBu",
                "organization": {
                    "ISIC": [
                        {
                            "context": "organization",
                            "dataSourceIdentifier": null,
                            "dateModified": "2017-11-14T15:32:07Z",
                            "fieldType": "http://schema.org/isicV4",
                            "label": null,
                            "name": "Economic Activity",
                            "ownerIdentifier": "AV-7KK2z2uFXwMzLKdBu",
                            "ownerType": "account",
                            "projectId": "DEMO",
                            "value": "G762"
                        }
                    ]
                },
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/accounts/AV-7KK2z2uFXwMzLKdBu"
                    },
                    "accounts": {
                        "href": "http://localhost:8084/my-project/accounts{?filter}",
                        "templated": true
                    },
                    "account-segments": {
                        "href": "http://localhost:8084/my-project/accounts/AV-7KK2z2uFXwMzLKdBu/account-segments{?filter}",
                        "templated": true
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/accounts?page=0&size=1"
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

Creation of new Accounts or Update of Accounts manually is not supported. Accounts are automatically
generated and updated from the Field Chunks sent by the different Connectors.

Deletion of existing Accounts is not allowed for now either. 

Navigating through the list of accounts, the link to each account can be found with the rel `self`. 

</article>

<article id="3">

## Filtering and Sorting Accounts Collection

Accounts collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Accounts from Madrid: `?filter=(organization/location/value eq 'Madrid')`
* Accounts with yearly revenue greater than 50000: `?filter=(organization/yearlyRevenue/value gt '50000')` 
* Accounts from Madrid sorted by yearlyRevenue in descending order:`?filter=(organization/location/value eq 'Madrid')&sort=organization/yearlyRevenue/value,desc` 
* Accounts in the financial sector located either in Malaga or Madrid with a yearly revenue lower than 50000:`?filter=(organization/location/value eq 'Madrid' or organization/location/value eq 'Malaga') and (organization/yearlyRevenue/value lt '50000') and (organization/isicV4/value eq 'K6419')`

</article>


<article id="4">

## Accounts Segments

As part of the links of each account, the following links can be found using these keys:
* `account-segments` - The collection of Account Segments this account belongs to
* `accounts` - The collection of Accounts

</article>
