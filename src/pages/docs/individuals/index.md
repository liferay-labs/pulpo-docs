---
title: "Individuals"
description: "Individuals API."
layout: "guide"
icon: "person"
weight: 6
---

###### {$page.description}

<article id="individual-model">

## The Individual Model

Individuals are a complex entity that contain several contexts for each Person
stored in our system. 

Those contexts are:
* Identity & Demographics
* Topics of Interest
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
* *dataSourceIndividualPKs* - a map with the different datasource identifiers as keys and the primary keys of this individual in each datasource. Potentially, an individual could be the aggregation of several users in one datasource. 
* *dateCreated*
* *dateModified*
* *identifier*
* *demographics* - a Map of identity & demographic [Fields](/docs/fields) corresponding to properties of the Type [Person](http://schema.org/Person) from Schema.org
* *interests* - a Map of interest [Fields](/docs/fields)

</article>


<article id="invidivuals-collection">

## Individuals Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `individuals` pointing to the
collection of Individuals.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting), [filtering](/docs/general#filtering) and [transformations](/docs/general#transformations).

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
                "interests": {
                    "digital experiences": [
                        {
                            "ownerIdentifier": "AWJEx5uAxvlVqtdUeNi",
                            "dataSourceIdentifier": "AWI_0f2_q_9uZvuIRBN3",
                            "fieldType": "http://schema.org/Number",
                            "ownerType": "individual",
                            "context": "interests",
                            "name": "digital experiences",
                            "value": "7.601294274664403",
                            "dateModified": "2018-03-27T10:57:37+0000",
                            "label": null,
                            "identifier": "AWJnGyoj8HyMTOCFNH"
                        }
                    ],
                    "modern portals": [
                        {
                            "ownerIdentifier": "AWJEx5uAxvlVqtdUeNi",
                            "dataSourceIdentifier": "AWI_0f2_q_9uZvuIRBN3",
                            "fieldType": "http://schema.org/Number",
                            "ownerType": "individual",
                            "context": "interests",
                            "name": "modern portals",
                            "value": "49.39764376820623",
                            "dateModified": "2018-03-27T10:57:37+0000",
                            "label": null,
                            "identifier": "AWJnGzAj8HyMTOCFNIC"
                        }
                    ]    
                },
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

<article id="filtering-individuals">

## Filtering and Sorting Individuals Collection

Individuals collection can be filtered as explained in [filtering](/docs/general#filtering).

These are some examples of filtering:
* Individuals from Madrid: `?filter=(demographics/city/value eq 'Madrid')`
* Individuals under 30 years old: `?filter=(demographics/age/value lt '30')` 
* Individuals from Madrid sorted by age in descending order:`?filter=(demographics/city/value eq 'Madrid')&sort=demographics/age/value,desc` 
* Individuals who work as Engineers from Malaga or Madrid under 40 years old:`?filter=(demographics/city/value eq 'Madrid' or demographics/city/value eq 'Malaga') and (demographics/age/value lt '30') and (demographics/jobTitle/value eq 'Engineer')`
* Individuals who work as Engineers from Madrid with an interest for Liferay:`?filter=(demographics/city/value eq 'Madrid' and demographics/jobTitle/value eq 'Engineer' and interests/liferay/value gt '0')`

### Filtering Individuals by Activities

You can obtain the individuals that performed an specific [activity](/docs/activities) on a time interval. For example, for an activity
with the key `forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057` (i.e. submit a form with id 32cf039a-7a47-4461-82c5-e694d9f29057):

* Individuals that submitted the form today (between 00:00 and current time):`?filter=(activities/today eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`
* Individuals that submitted the form yesterday:`?filter=(activities/yesterday eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')` 
* Individuals that submitted the form within the last 7 days (excluding today):`?filter=(activities/last7days eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`
* Individuals that submitted the form within the last 30 days (excluding today):`?filter=(activities/last30days eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`
* Individuals that submitted the form within the last 90 days (excluding today):`?filter=(activities/last90days eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`
* Individuals that submitted the form within the last year (12 previous months):`?filter=(activities/lastYear eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`
* Individuals that ever submitted the form: `?filter=(activities/ever eq 'forms#formSubmitted#32cf039a-7a47-4461-82c5-e694d9f29057')`

Filtering by activities can be combined with any of the aforementioned filters.

### Filtering Individuals by Segment Identifiers
You can obtain the individuals that belong or do not belong to specific [Individual Segments](/docs/individual_segments). For example, for Individual Segments with identifiers `32cf039a-7a47-4461-82c5-000000000001` and `32cf039a-7a47-4461-82c5-000000000002`.

* Individuals that belong to the first segment:`?filter=(individualSegmentIdentifiers eq '32cf039a-7a47-4461-82c5-000000000001')`
* Individuals that do not belong to the first segment:`?filter=(individualSegmentIdentifiers ne '32cf039a-7a47-4461-82c5-000000000001')`
* Individuals that belong to the first segment and not to the second segment:`?filter=(individualSegmentIdentifiers eq '32cf039a-7a47-4461-82c5-000000000001' and individualSegmentIdentifiers ne '32cf039a-7a47-4461-82c5-000000000002')`

Filtering by Individual Segments can be combined with any of the aforementioned filters.

</article>


<article id="individual-transformations">

## Transformations on Individuals Collection

Transformations can be applied on Individuals collection as explained in [transformations](/docs/general#transformations).

The only transformation allowed for the Individuals Collection is `groupby` by a field value.

These are some examples of transformations:
* Individuals grouped by address: `?apply=groupby((demographics/address/value))`

This is an example of a response to this url: `http://localhost:8084/my-project/individuals?apply=groupby((demographics/address/value))page=0&size=20`

```json
{
  "_embedded": {
    "individual-transformations": [
      {
        "totalElements": 1,
        "terms": {
          "demographics/address/value": "candelaria"
        },
        "_links": {
          "individuals": {
            "href": "http://localhost:8084/DEMO/individuals?filter=(demographics/address/value%20eq%20%27candelaria%27){&page,size,sort*}"
          }
        }
      },
      {
        "totalElements": 2,
        "terms": {
          "demographics/address/value": "malaga"
        },
        "_links": {
          "individuals": {
            "href": "http://localhost:8084/DEMO/individuals?filter=(demographics/address/value%20eq%20%27malaga%27){&page,size,sort*}"
          }
        }
      }
    ]
  },
  "_links": {
    "self": {
      "href": "http://localhost:8084/DEMO/individuals?apply=groupby((demographics/address/value))&page=0&size=20"
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

Aggregations of individuals by a field are by default sorted by the number of individuals grouped 
(aggregations with a higher number of individuals first), but they can also be
ordered by the value of the field used to group by (See [sorting](/docs/general#sorting)). 

These are some examples of valid sortings: 
- ?sort=demographics/address/value
- ?sort=demographics/jobTitle/value,desc

</article>


<article id="individual-segments">

## Individual Segments

As part of the links of each individual, the following links can be found using these keys:
* `individual-segments` - The collection of Individual Segments this individual belongs to
* `individuals` - The collection of Individuals

</article>

<article id="individual-embedded">

## Embedded Resources on an Individual Collection

The `expand` parameter can be used when retrieving a Collection of Individuals as explained in [embedding resources](/docs/general#embedding-resources).

The supported resources that can be embedded inside each individual resource are:

* *data-sources* - The Data Sources contributing information to Fields
belonging to the Individual
* *individual-segments* - The Individual Segments the individual belongs
to
</article>

<article id="individual-indivisual-segments-embedded">

## Embedded resources on Individual Segments of an Individual 

The `expand` parameter can also be used when retrieving [Individuals Segments](/docs/individuals#individual-segments) of an Individual as explained in [embedding resources](/docs/general#embedding-resources).
The supported resource that can be embedded inside each individual segment resource is:

* *active-membership* - The **active** [Individual Segment Membership](/docs/individual_segments#individual-segment-membership-model) 
that associates the current Individual and Individual Segment.


This is an example of a response to this url: `http://localhost:8084/my-project/individuals/AWRAgrHwCx5q5cIjnRhC/individual-segments?expand=active-membership&page=0&size=10&sort=name,asc`

```json
{
    "_embedded": {
        "individual-segments": [
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN"
                    },
                    "individual-segments": {
                        "href": "http://localhost:8084/my-project/individual-segments{?filter,page,size,sort*}"
                    },
                    "individuals": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN/individuals{?apply,filter,page,size,sort*}"
                    },
                    "memberships": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN/memberships{?filter,page,size,sort*}"
                    },
                    "membership-changes": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN/membership-changes{?apply,filter,page,size,sort*}"
                    }
                },
                "_embedded": {
                    "active-membership": {
                        "_links": {
                            "self": {
                                "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN/memberships/AWRAgrHwCx5q5cIjnRhC"
                            },
                            "individual": {
                                "href": "http://localhost:8084/my-project/individuals/AWRAgrHwCx5q5cIjnRhC{?expand}",
                                "templated": true
                            },
                            "individual-segment": {
                                "href": "http://localhost:8084/my-project/individual-segments/AWRAgrPhCx5q5cIjnRhN"
                            }
                        },
                        "status": "ACTIVE",
                        "individualIdentifier": "AWRAgrHwCx5q5cIjnRhC",
                        "dateCreated": "2018-06-27T09:11:05Z",
                        "individualSegmentIdentifier": "AWRAgrPhCx5q5cIjnRhN"
                    }
                },
                "name": "Developers",
                "state": "READY",
                "individualCount": 1,
                "dateModified": "2018-06-27T09:11:04Z",
                "identifier": "AWRAgrPhCx5q5cIjnRhN",
                "scope": "PROJECT",
                "status": "ACTIVE",
                "segmentType": "DYNAMIC",
                "dateCreated": "2018-06-27T09:11:04Z",
                "filter": "(demographics/jobTitle/value eq 'Dev')"
            },
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS"
                    },
                    "individual-segments": {
                        "href": "http://localhost:8084/my-project/individual-segments{?filter,page,size,sort*}"
                    },
                    "individuals": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS/individuals{?apply,filter,page,size,sort*}"
                    },
                    "memberships": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS/memberships{?filter,page,size,sort*}"
                    },
                    "membership-changes": {
                        "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS/membership-changes{?apply,filter,page,size,sort*}"
                    }
                },
                "_embedded": {
                    "active-membership": {
                        "_links": {
                            "self": {
                                "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS/memberships/AWRAgrHwCx5q5cIjnRhC"
                            },
                            "individual": {
                                "href": "http://localhost:8084/my-project/individuals/AWRAgrHwCx5q5cIjnRhC{?expand}",
                                "templated": true
                            },
                            "individual-segment": {
                                "href": "http://localhost:8084/my-project/individual-segments/AWRAgrROCx5q5cIjnRhS"
                            }
                        },
                        "status": "ACTIVE",
                        "individualIdentifier": "AWRAgrHwCx5q5cIjnRhC",
                        "dateCreated": "2018-06-27T09:11:05Z",
                        "individualSegmentIdentifier": "AWRAgrROCx5q5cIjnRhS"
                    }
                },
                "name": "Wealthy",
                "state": "READY",
                "individualCount": 1,
                "dateModified": "2018-06-27T09:11:05Z",
                "identifier": "AWRAgrROCx5q5cIjnRhS",
                "scope": "PROJECT",
                "status": "ACTIVE",
                "segmentType": "DYNAMIC",
                "dateCreated": "2018-06-27T09:11:05Z",
                "filter": "(demographics/salary/value eq '5000')"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/individuals/AWRAgrHwCx5q5cIjnRhC/individual-segments?expand=active-membership&page=0&size=10&sort=name,asc"
        }
    },
    "page": {
        "size": 10,
        "totalElements": 2,
        "totalPages": 1,
        "number": 0
    }
}
```

</article>


<article id="creating-individuals">

## Creating Individuals

Individuals are not created using the REST API. Dependending on the data source, 
the individuals should be created differently:

### Creating Individuals from Liferay DataSource

To send Users from a Liferay Server to Pulpo, you need to have a [LCS configured environment](https://customer.liferay.com/documentation/7.0/deploy/-/official_documentation/deployment/using-lcs) and also you should install the following OSGi modules to your Liferay Server:

- `com.liferay.pulpo:com.liferay.pulpo.connector.de.contacts.api`
- `com.liferay.pulpo:com.liferay.pulpo.connector.de.contacts.impl`

If the installation is OK, every time a User is stored/changed in the DB, all the information 
related with this User will be sent to the Pulpo engine.

#### How to add extra information to the User

To Serialize custom fields of the User, you just need to register a CustomFieldSerializer OSGI service. Where:
* `getCustomField`: in this method you should return the object that contains the extra information of the user.
* `getCustomFieldClass`: in this method you should return the class of the object returned by `getCustomField`. 
* `getCustomFieldName`: name that will be used to serialize the object returned by `getCustomField`.
* `writeAsString`: serialization of the object return by `getCustomField` as a valid JSON Object.

This is an example of a implementation of CustomFieldSerializer.

```java
@Component(immediate = true, service = CustomFieldSerializer.class)
public class CustomFieldExampleSerializer
    implements CustomFieldSerializer<CustomFieldExample> {

    @Override
    public CustomFieldExample getCustomField(User user) {
        ...
    }

    @Override
    public Class getCustomFieldClass() {
        ...
    }

    @Override
    public String getCustomFieldName(){
        ...
    }

    @Override
    public String writeAsString(T object){
        ...
    }

	}
```

### Creating Individuals from a CSV DataSource
* `csv_pulpo/individual_chunk_add_<environment_name>` - The queue to write
messages to when creating individuals via CSV import. `<environment_name>` may
be one of `dev`, `pre` or `prod`.

The messages written to this queue, are expected to have the following format:

```json
{
	"projectId" : "<projectId>",
	"dataSourceIdentifier" : "<dataSourceIdentifier>",
	"individualSegmentIdentifiers" : "<individualSegmentIdentifiers>",
	"fields" : {
		"name" : "value",
		...
	}
}
```

Where:
- `projectId` is your LCS projectId.
- `dataSourceIdentifier` is your DataSource identifier.
- `individualSegmentIdentifiers` is a optional field and should be a JSON array of one or several individualSegmentIdentifiers.
- `fields` is a optional field and should be a JSON object where each pair name/value should be mapped as columnName/columnValue.

It is also possible to send batches of that kind of messages:

```json
[
	{
		"projectId" : "<projectId>",
		"dataSourceIdentifier" : "<dataSourceIdentifier>",
		"individualSegmentIdentifiers" : "<individualSegmentIdentifiers>",
		"fields" : {
			"name" : "value",
			...
		}
	},
	{
		"projectId" : "<projectId>",
		"dataSourceIdentifier" : "<dataSourceIdentifier>",
		"individualSegmentIdentifiers" : "<individualSegmentIdentifiers>",
		"fields" : {
			"name" : "value",
			...
		}
	}
]
```

</article>
