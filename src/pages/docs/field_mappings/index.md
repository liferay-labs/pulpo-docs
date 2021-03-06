---
title: "Field Mappings"
description: "Field Mappings API"
layout: "guide"
icon: "download"
weight: 5
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
* *author*
  * identifier
  * name
* *strategy* - the strategy used to map fields. For example, we could decide to use always the most recent value of a field, or 
give preference to the value from a specific data source. See more details below.

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

Adding new data Source Field Names without having to update the full field mapping
is also supported using the `PATCH` method to an entity link.
The body must contain a Data Source Field Name object, wich contains the following fields:
* dataSourceIdentifier - the identifier of an existing datasource.
* fieldName - the name of this field in the existing data source

This is an example of the body to patch an existing Field Mapping in order to add
another data source field name:

```json
{
	"dataSourceIdentifier" : "AV-0-c1_4MMBozrmZ0B",
	"fieldName" : "years-old"	
}
```
</article>

<article id="2">

## Field Mappings Strategies

Field Mapping Strategies have the following fields:

* *key* - the stratey key. Supported values are: "MOST_RECENT" and "PRIORITY_DATASOURCE"
* *configuration* - A map with the specific configuration for the strategy. e.g. Most Recent doesn't need
any configuration. However, Priority DataSource requires the value dataSourceIdentifier.

#### Most Recent Strategy

This strategy will add to the user fields the most recent field coming from any
data source. For example, if we have two data sources set up and a field 
mapping configured to obtain the email address from both. If we only receive the
email from one of them, that is the field that will be added to the individual profile.
However, if we receive both, then the one which we received the latest will be the
one added to the individual profile.  

This is an example of a valid strategy passed in JSON when creating or updating a
field mapping to use the most recent field: 

```json
{
    "key": "MOST_RECENT"
}
```

#### Priority Data Source Strategy

This strategy will give preference to the information coming from a particular 
data source. In order to be configured, the configuration must include the
dataSourceIdentifier. For example, if we have three data sources set up (A, B and C), and
a field mapping configured to obtain the telephone from all of them. When we set 
A as the priority data source, if we receive a telephone field from A, then that 
will be the field added to the inidividual profile. In case we don't receive any
telephone from A, but we do receive it from B and C, then the most recent strategy
applies.

And this is an example setting the data source with identifier ABCEDFG as the 
priority data source.

```json
{
    "key": "PRIORITY_DATASOURCE",
    "configuration": {
        "dataSourceIdentifier": "ABCDEFG"
    }
}
```
</article>