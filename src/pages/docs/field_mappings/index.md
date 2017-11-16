---
title: "Field Mappings"
description: "Field Mappings API"
layout: "guide"
icon: "download"
weight: 4
---

###### {$page.description}

<article id="1">

## The Field Mapping Model

Field Mappings allow us to map the fields from the different [datasources](/docs/datasources) to the
[fields](/docs/fields) in our entities ([individuals](/docs/individuals), Accounts... etc)

The following fields are currently supported in a field mapping:
* *context* - the context of the field (demographics, sentiment... etc)
* *dataSourceFieldNames* - a map with the name of the original field for each Data Source.

	```
	e.g. 
	     liferay --> phone
	     salesforce --> tel
	```

* *dateCreated*
* *dateModified*
* *fieldName* - the name of the field on our model
* *fieldType* - a property from schema.org associated to this field mapping. e.g. telephone (http://schema.org/telephone)
* *identifier*


</article>


<article id="2">

## Field Mappings Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `field-mappings` pointing to the
collection of Field Mappings.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of fields
under the key `field-mappings`.

This is an example of a response to this url: http://localhost:8084/my-project/field-mappings?page=0&size=1

```json
{
    "_embedded": {
        "field-mappings": [
            {
                "context": "demographics",
                "dataSourceFieldNames": {
                    "liferay_AV-0-c1_4MMBozrmZ0T_": "age",
                    "salesforce_AV-0-c4v4MMBozrmZ0UA": "years"
                },
                "dateCreated": "2017-11-13T10:43:11Z",
                "dateModified": "2017-11-13T10:43:11Z",
                "fieldName": "age",
                "fieldType": "http://schema.org/age",
                "identifier": "AV-0-dAM4MMBozrmZ0UD",
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/field-mappings/AV-0-dAM4MMBozrmZ0UD"
                    },
                    "field-mappings": {
                        "href": "http://localhost:8084/my-project/field-mappings{?filter}",
                        "templated": true
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/field-mappings?page=0&size=20"
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

Creation of new Field Mappings is supported making a `POST` to the Collection URL. This is
an example of the body passed to this POST request: 

```json
{
    "context": "demographics",
    "dataSourceFieldNames": {
        "AV-0-c1_4MMBozrmZ0T_": "age",
        "AV-0-c4v4MMBozrmZ0UA": "years"
    },
    "fieldName": "age",
    "fieldType": "http://schema.org/age"
}
```

Navigating through the list of entities, the link to each entity can be found with the rel `self`. 
That same url can be also used for delete (`DELETE` method) and update (`PUT` method).

</article>