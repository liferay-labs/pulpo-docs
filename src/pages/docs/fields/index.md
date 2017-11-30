---
title: "Fields"
description: "Fields API"
layout: "guide"
icon: "menu"
weight: 3
---

###### {$page.description}

<article id="1">

## The Field Model

Fields contain information about different entities ([individuals](/docs/individuals), accounts, individual segments... etc).

All the different values in time for a field are stored as fields themeshelves.
Therefore, if the field Address for an Individual changed 5 times, we will have
5 fields for the Address for that particular Individual.

The entity field contains the following fields:
* *identifier*
* *context*
* *dataSourceIdentifier*
* *dateModified*
* *fieldType* - A schema.org property
* *label*
* *name*
* *value*
* *ownerType* - The entity that owns this field (e.g. an Individual, an Individual Identifier...)
* *ownerIdentifier* - The Identifier of the entity that owns this field.

</article>


<article id="2">

## Fields Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `fields` pointing to the
collection of Fields.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of fields
under the key `fields`.

This is an example of a response to this url: http://localhost:8084/my-project/fields?page=0&size=1

```json
{
    "_embedded": {
        "fields": [
            {
                "context": "demographics",
                "dataSourceIdentifier": "AV-0-c1_4MMBozrmZ0T_",
                "dateModified": "2017-11-13T10:43:13Z",
                "fieldType": "http://schema.org/telephone",
                "identifier": "AV-0-deU4MMBozrmZ0UO",
                "label": "home",
                "name": "telephone",
                "ownerIdentifier": "AV-0-dcI4MMBozrmZ0UM",
                "ownerType": "individual",
                "value": "+34699001234",
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/fields/AV-0-deU4MMBozrmZ0UO"
                    },
                    "individual": {
                        "href": "http://localhost:8084/my-project/individuals/AV-0-dcI4MMBozrmZ0UM"
                    },
                    "fields": {
                        "href": "http://localhost:8084/my-project/fields{?filter}",
                        "templated": true
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/fields?page=0&size=1"
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

Creation of new Fields or Update of Fields manually is not supported. Fields are automatically
generated and updated from the Field Chunks sent by the different Connectors.

Deletion of existing Fields is not allowed for now either. 

Navigating through the list of fields, the link to each field can be found with the rel `self`. 

</article>