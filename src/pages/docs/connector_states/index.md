---
title: "Connector States"
description: "Connector States API"
layout: "guide"
icon: "link"
weight: 13
---

###### {$page.description}

<article id="1">

## The Connector State Model

Connector State contains information about the state of an Analytics Cloud connector installed on a remote Liferay instance connected to LCS.

The entity asset contains the following fields:
* *identifier* - the unique identifier of the connector state
* *installationId* - the ID of the system where the connector is installed 
* *state* - the current state of the connector. Possible values are: `READY` and `OFFLINE`
* *dateCreated* - the date when the connector state was created. 
* *dateModified* - the date when the connector state was modified.

</article>

<article id="2">

## Connector States Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `connector-states` pointing to the
collection of Connector States.

This API supports [pagination](/docs/general#pagination) and [sorting](/docs/general#sorting).

The response will contain inside the `_embedded` section, a list of connector states
under the key `connector-states`.

This is an example of a response to this url: http://localhost:8084/my-project/connector-states?page=0&size=1

```json
{
    "_embedded": {
        "connector-states": [
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/connector-states/my-installation-id"
                    },
                    "connector-states": {
                        "href": "http://localhost:8084/my-project/connector-states{?page,size,sort*}"
                    }
                },
                "state": "READY",
                "identifier": "AWQ73UnelLXzRA3LpnPi",
                "dateModified": "2018-06-26T11:31:55Z",
                "installationId": "my-installation-id",
                "dateCreated": "2018-06-26T11:31:55Z"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/my-project/connector-states?page=0&size=1"
        }
    },
    "page": {
        "size": 1,
        "totalElements": 1,
        "totalPages": 1,
        "number": 0
    }
}
```

Creation or Update of Connector States is not supported by the API. Connector States are automatically
generated and updated from the Heartbeat messages sent by the installed connectors.

Deletion of existing Connector States is not allowed either. 

Navigating through the list of connector states, the link to each connector state can be found with the rel `self`. 

</article>