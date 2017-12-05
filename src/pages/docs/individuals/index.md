---
title: "Individuals"
description: "Individuals API."
layout: "guide"
icon: "person"
weight: 6
---

###### {$page.description}

<article id="1">

## The Individual Model

Individuals are a complex entity that contain several contexts for each Person
stored in our system. 

Those contexts are:
* Identity & Demographics
* Sentiment Analysis
* Event Attendance
* Sales Interaction
* Social Engagement
* Transactions
* Browsing Activity
* Firmographic
* Support Interaction
* etc
 
Only some of those contexts are available for now. Each context will contain
certain [Fields](/docs/fields) with information. These fields will correspond to [schema.org](http://schema.org/) Properties

The following fields are currently supported as part of an Invididual:
* *dataSourceIdentifiers* - a map with the different datasource identifiers and the
   datasource individual identifier (the primary key of this individual in
   the original datasource) 
* *dateCreated*
* *dateModified*
* *identifier*
* *demographics* - a Map of identity & demographic [Fields](/docs/fields) corresponding to properties of the Type [Person](http://schema.org/Person) from Schema.org
* *sentiment* - a Map of sentiment analysis [Fields](/docs/fields)

</article>


<article id="2">

## Individuals Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `individuals` pointing to the
collection of Individuals.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of individuals
under the key `individuals`.

This is an example of a response to this url: `http://localhost:8084/my-project/individuals?page=0&size=20`

```json
{
    "_embedded": {
        "individuals": [
            {
                "dateCreated": "2017-11-13T11:47:43Z",
                "dateModified": "2017-11-13T11:47:44Z",
                "demographics": {
                    "address": [
                        {
                            "context": "demographics",
                            "dataSourceIdentifier": "AV-1NOAPDh9K2u0PkWnD",
                            "dateModified": null,
                            "fieldType": "http://schema.org/address",
                            "identifier": null,
                            "individualIdentifier": "AV-1NOYHDh9K2u0PkWnL",
                            "label": null,
                            "name": "address",
                            "projectId": "my-project",
                            "value": "125 Main Street, Candelaria"
                        }
                    ],
                    "telephone": [
                        {
                            "context": "demographics",
                            "dataSourceIdentifier": "AV-1NN9zDh9K2u0PkWnC",
                            "dateModified": null,
                            "fieldType": "http://schema.org/telephone",
                            "identifier": null,
                            "individualIdentifier": "AV-1NOYHDh9K2u0PkWnL",
                            "label": "home",
                            "name": "telephone",
                            "projectId": "my-project",
                            "value": "+34699001234"
                        }
                    ],
                    "email": [
                        {
                            "context": "demographics",
                            "dataSourceIdentifier": "AV-1NOAPDh9K2u0PkWnD",
                            "dateModified": null,
                            "fieldType": "http://schema.org/email",
                            "identifier": null,
                            "individualIdentifier": "AV-1NOYHDh9K2u0PkWnL",
                            "label": null,
                            "name": "email",
                            "projectId": "my-project",
                            "value": "cris@liferay.com"
                        }
                    ],
                    "age": [
                        {
                            "context": "demographics",
                            "dataSourceIdentifier": "AV-1NN9zDh9K2u0PkWnC",
                            "dateModified": null,
                            "fieldType": "http://schema.org/age",
                            "identifier": null,
                            "individualIdentifier": "AV-1NOYHDh9K2u0PkWnL",
                            "label": null,
                            "name": "age",
                            "projectId": "my-project",
                            "value": "32"
                        }
                    ]
                },
                "identifier": "AV-1NOYHDh9K2u0PkWnL",
                "sentiment": {},
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/individuals/AV-1NOYHDh9K2u0PkWnL"
                    },
                    "individuals": {
                        "href": "http://localhost:8084/my-project/individuals{?filter}",
                        "templated": true
                    },
                    "individual-segments": {
                        "href": "http://localhost:8084/my-project/individuals/AV-1NOYHDh9K2u0PkWnL/individual-segments"
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/individuals?page=0&size=20"
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

Creation of new Individuals or Update of Individuals manually is not supported. Indivuals are automatically
generated and updated from the Field Chunks sent by the different Connectors.

Deletion of existing Individuals is not allowed for now either. 

Navigating through the list of individuals, the link to each individual can be found with the rel `self`. 

</article>

<article id="3">

## Filtering and Sorting Individuals Collection

Individuals collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individuals from Madrid: `?filter=(demographics/city/value eq 'Madrid')`
* Individuals under 30 years old: `?filter=(demographics/age/value lt '30')` 
* Individuals from Madrid sorted by age in descending order:`?filter=(demographics/city/value eq 'Madrid')&sort=demographics/age/value,desc` 
* Individuals who work as Engineers from Malaga or Madrid under 40 years old:`?filter=(demographics/city/value eq 'Madrid' or demographics/city/value eq 'Malaga') and (demographics/age/value lt '30') and (demographics/jobTitle/value eq 'Engineer')`

</article>


<article id="4">

## Individual Segments

As part of the links of each individual, the following links can be found using these keys:
* `individual-segments` - The collection of Individual Segments this individual belongs to
* `individuals` - The collection of Individuals

</article>
