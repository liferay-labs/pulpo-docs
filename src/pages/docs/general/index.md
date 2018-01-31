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

Any request to this API will have to be signed, otherwise you will receive a 401 error response. 

In order to have a signed request, it must contain the following http headers:
- *timestamp*: the timestamp of the request in milliseconds
- *apikey*: your client key for accessing this API (ask for one to the Pulpo Team)
- *signature*: your request signed. In order to sign your request, you can use the SignatureGenerator utility to obtain this signature.

### Signature Generation

You can use the following gradle dependency to use the `SignatureValidator.java` utility:
    
```
provided group: "com.liferay.osb.pulpo", name: "com.liferay.osb.pulpo.engine.contacts.client", version: "0.0.1-20180131.153601-1"
```


In order to obtain the signature for your request, you can use one of the following methods:
- `SignatureGenerator.getSignature(HttpServletRequest request, String private key)` 
  - This will be helpful if you already have the request object. It must contain the timestamp and api key headers. If for any reason, you don't have access to the request, you can use the following method:

- `SignatureGenerator.getSignature(String requestURL, Map<String, String[]> parameters, String apiKey, String timeStamp, String privateKey)`
  - The request URL must be the same URL you are making your request to (it may include url parameters as well)
  - The parameters map must contain the parameters of the body of your request (if any)
  - The timestamp must match the value in your timestamp header
  - The api key must match your api key header too.

In both methods, you are expected to pass the private key you received associated to your api key. If you don't pass any private key, it will automatically look for the system variable `PULPO_PRIVATE_KEY`, therefore, you can just set this variable and don't pass any privateKey to the method.

This is an example of how the request headers could look like:

```
"GET /DEMO/accounts?filter=(organization/isicV4/value eq 'G1252') HTTP/1.1"
header: "Accept: */*"
header: "apikey: MY-API-KEY"
header: "timestamp: 1517245158236"
header: "signature: MC0C1234-ew5bytvbSxebxAhUAg57SDhuBIGmJkS45zo"
```

Signatures are unique to each combination of url, params and headers, therefore they need to be generated for each request, they can't be reused.

This is an example of how to generate the signature for a POST request:

*1) Include the api key Heade in your request
 ```
 header: "apikey: MY-API-KEY"
 ```
*2) Include the timestamp Header in your request
```
header: "timestamp: 1517245158236"
```
(e.g. `System.currentTimeMillis()` in Java)

*3) Obtain the Signature. If we had the http request, we would just call `SignatureGenerator.getSignature(HttpServletRequest request)`, and include that as a third header:
```
header: "signature: (result of the previous call)"
```

*3b) In case you didn't have the http request, we would call:
```
SignatureGenerator.getSignature(String requestURL, Map<String, String[]> parameters, String apiKey, String timeStamp, String privateKey)
```
with these values:

* requestURL --> /DEMO/accounts?filter=(organization/isicV4/value eq 'G1252')
* parameters --> empty
* apikey --> the same value from the header
* timeStamp --> the same value from the header
* privateKey --> the private key associated to our api key

Another example of a POST request with body parameters:

* requestURL --> /DEMO/datasources
* parameters --> Map with the parameters: 
```
  {
    "name" : ["My DataSource"],
    "url" : ["My DataSource URL"]
  }
```

* apikey --> the same value from the header
* timeStamp --> the same value from the header
* privateKey --> the private key associated to our api key

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

