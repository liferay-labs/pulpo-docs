---
title: "General"
description: "General API"
layout: "guide"
icon: "star"
weight: 1
---

###### {$page.description}

<article id="authentication">

## Authentication

Any request to this API will have to be signed, otherwise you will receive a 401 error response. Learn more about how to [authenticate your requests](/docs/general/authentication.html) . 

</article>

<article id="navigation">

## Initial Navigation to obtain URLs

URLs are not part of this API, they may change at any moment. URLs must be asked to
the service before making any request.

The URLs can be obtained making a request to the root resource of the Service `/`.

The response in `json HAL` format will contain a `_links` object with the different template links to be used. For example: 

```json
{
    "name": "pulpo-api",
    "description": "API for consuming PULPO Services",
    "_links": {
        "self": {
            "href": "http://localhost:8084/"
        },
        "data-sources": {
            "href": "http://localhost:8084/{projectId}/data-sources{?filter}",
            "templated": true
        },
        "field-mappings": {
            "href": "http://localhost:8084/{projectId}/field-mappings{?filter}",
            "templated": true
        },
        "fields": {
            "href": "http://localhost:8084/{projectId}/fields{?filter}",
            "templated": true
        },
        "individuals": {
            "href": "http://localhost:8084/{projectId}/individuals{?filter}",
            "templated": true
        },
        "individual-segments": {
            "href": "http://localhost:8084/{projectId}/individual-segments{?filter}",
            "templated": true
        }
    }
}
```

The template URLs for managing collections such as Data Sources or Individuals can be
found inside the section `_links` with the keys `data-sources` or `individuals`.
(These keys are our API and they will never change). 

These template URLs allow us to build ULRs that can always be used to obtain the entities (`GET` method) and
create new ones (`POST` method). (Note that not all collections support creation of
new objects. e.g. Individuals API doesn't allow to create individuals directly).

These template URLs need certain items to be replaced in order
to have a valid URL:
* variables: `{lb}parameterName{rb}` They should be replaced with a value. e.g. The {lb}projectId{rb} item must be replaced with the projectId of the current proje`ct (such as "my-project").
* parameters: `{lb}?parameterName{rb}` They should be replaced with a param and a value. e.g. The {lb}?filter{rb} item must be replaced with a filter parameter and as value a valid oData filter or with an empty string. (e.g. `&filter=(name eq 'Jon')`)

Important: Optional parameters can be added at any time to these APIs, therefore, clients must
consider that the templates may change with additional optional parameters (never with
mandatory parameters). 

Navigating through a collection of entities, the link to each entity can be found with the rel `self`. 
That same url can be also used for delete (`DELETE` method) and update (`PUT` method).
</article>

<article id="pagination">

## Pagination

Every collection URL can be paginated using the optional params `page` and `size`.

This is an example of a response to this url: http://localhost:8084/my-project/data-sources?page=0&size=1

```json
{
    "_embedded": {
        "data-sources": [
            {
                "dateCreated": "2017-09-14T12:00:04Z",
                "dateModified": "2017-09-14T12:00:04Z",
                "identifier": "AV6AQqVHWUV1yhbro9xD",
                "name": "my Liferay 6.2",
                "provider": {
                    "name": "liferay-de"
                },
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/data-sources/AV6AQqVHWUV1yhbro9xD"
                    },
                    "data-sources": {
                        "href": "http://localhost:8084/my-project/data-sources"
                    }
                }
            }
        ]
    },
    "_links": {
        "first": {
            "href": "http://localhost:8084/my-project/data-sources?page=0&size=1"
        },
        "self": {
            "href": "http://localhost:8084/my-project/data-sources?page=0&size=1"
        },
        "next": {
            "href": "http://localhost:8084/my-project/data-sources?page=1&size=1"
        },
        "last": {
            "href": "http://localhost:8084/my-project/data-sources?page=1&size=1"
        }
    },
    "page": {
        "size": 1,
        "totalElements": 2,
        "totalPages": 2,
        "number": 0
    }
}
```

From this response, you can obtain the total number of existing elements under the `page` block and also
the links to other pages of data sources.

</article>

<article id="sorting">

## Sorting

Every collection URL can be sorted using the optional param `sort`.

e.g. Given a url for a Collection (such as http://localhost:8084/my-project/data-sources)
I could sort the results by name appending to the url `?sort=name`

In order to navigate inside the fields of an entity, you should use the separator `/`
e.g. I could sort the results by the author name appending `?sort=author/name`

By default, the sorting is in ascending order. (0-1-A-Z). However, this can be changed
adding `desc` after the parameter name separated with a comma.

e.g. I could sort the results by name descending, appending to the url `?sort=name,desc`

I could also sort by more than one field, adding more than one sort parameter or separating the fields by commas.
In this situation, the first parameter found is used to sort, and in case of coincidence,
the next parameter in the list is used to sort and so on. 

e.g. I could sort the results by the name of the Provider, and in case of coincidence
then order by the Date of creation appending this to the url: `?sort=provider/name,dateCreated`

If I want to change the order to descending for one of the fields, then I must 
used separated parameters in this way:

`?sort=provider.name,desc&sort=dateCreated,asc`

</article>

<article id="filtering"> 

## Filtering

Not all collections allow filtering. The ones that support it will contain the 
optional parameter `{lb}?filter{rb}` in their template.

In order to filter a collection based on the value of one or more fields, you
can use the optional parameter filter following a subset of the oData standard.

#### Comparison Operators

| Operator  | Description          | Example                      |
|---------- |--------------------- |------------------------------|
| eq        | Equal                | Address/City eq 'Redmond'    |
|           | Equal null           | Address/City eq null         |
| ne        | Not equal            | Address/City ne 'London'     |
|           | Not null             | Address/City ne null         |
| gt        | Greater than         | Price gt 20                  |
| ge        | Greater than or equal| Price ge 10                  |
| lt        | Less than            | Price lt 20                  |
| le        | Less than or equal   | Price le 100                 |

#### Logical Operators

| Operator  | Description | Example                      |
|---------- |------------ |------------------------------|
|and|Logical and |Price le 200 and Price gt 3.5|
|or |Logical or |Price le 3.5 or Price gt 200 |


#### Grouping Operators

| Operator  | Description | Example                      |
|---------- |------------ |------------------------------|
|( ) |Precedence grouping |(Price eq 5) or (Address/City eq 'London')  |

#### String functions

| Function  | Description | Example                      |
|---------- |------------ |------------------------------|
| contains  | Contains    |contains(Address/City,'edmon')|
|startswith |Starts with  |startswith(Address/City,'Red')|
|endswith   |Ends with    |endswith(Address/City,'mond') |

e.g. We could append this to a URL that returns a collection of Data Sources
to filter the DataSource by an author name and a name.
```
?filter=(author/name eq 'Julio') and (name ne 'datasource-name')
```

#### Escaping in queries:

In order to filter for a value which contains single quotes, these can
be escaped by adding two single quotes.

e.g. To filter for a company whose name is `L'Oreal`:
```
?filter=(company/name eq 'L''Oreal')
```

#### Filtering by interests:

Interests (of an individual or of a segment) may have values which can
contain characters not supported by Odata as Identifiers, as described
here:
http://docs.oasis-open.org/odata/odata/v4.0/errata03/os/complete/part3-csdl/odata-v4.0-errata03-os-part3-csdl-complete.html#_SimpleIdentifier

For example an interest may have a value `digital experience`, but the
following query would result in a parsing error:

```
?filter=(interests/digital experience/value gt '0')
```

To support filtering by such interests, the identifier is modified, so
that whitespace characters are replaced with `_` and not alpha numeric
characters are removed.
In order to filter by interest `digital experience` the correct query
would be:

```
?filter=(interests/digital_experience/value gt '0')
```

</article>

<article id="transformations">

## Transformations

Not all collections support transformations. The ones that support them will 
contain the optional parameter `{lb}?apply{rb}` in their template.

In order to perform a transformation on a collection, you can use the optional 
parameter `apply` following a subset of the oData standard.

Currently, the only supported transformation is `groupby` and within it, only 
`Simple grouping` on a *single field* is allowed, with no further 
transformations applied is supported.

#### Supported Transformations

| Tranformation | Description   | Example                               |
|---------------|---------------|---------------------------------------|
| groupby       | Group by      | groupby((demographics/address/value)) |

</article>

