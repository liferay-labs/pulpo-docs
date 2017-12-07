---
title: "Field Names"
description: "Field Names API"
layout: "guide"
icon: "filter"
weight: 4
---

###### {$page.description}

<article id="1">

## The Field Names

Sometimes, in order to choose an existing field mapping to map your own data, 
you may want to find the most appropriate field considering the information you
have. This service will help you with that considering the following information:
* Existing fields with the same or similar name of your label
* Previous field mappings from your label to other fields
* Fields with values similar to your new values

</article>


<article id="2">

## Fields Names Service

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `field-names` pointing to the
endpoint for obtaining the field names.

This endpoint accepts the `GET`	method with the following parameters:
 * label - the label from your field (e.g. telephone, email addres... etc)
 * ownerType - the type of entity your are mapping (individual, account...)
 * values - array of sample values from your field
   

The response will contain a list of existing field names ordered by relevance.

This is an example of a response to this url: http://localhost:8084/my-project/field-names?ownerType=individual&label=phone

```json
["telephone", "faxNumber", "globalLocationNumber"]
```
</article>