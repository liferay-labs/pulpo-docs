---
title: "Pages Visited"
description: ""Pages Visited API"
layout: "guide"
icon: "file"
weight: 10
---

###### {$page.description}

<article id="1">

## The Page Visited Model

Pages Visited contain information about pages visited by different entities ([individuals](/docs/individuals), accounts, individual segments... etc).

The entity page visited contains the following fields:
* *identifier*
* *day*
* *description* - description of the page
* *interestName* - The name of the interest which the visit of this page contribute to justify
* *ownerType* - The entity that visit the page (e.g. an Individual, an Account...)
* *ownerIdentifier* - The Identifier of the entity that visit the page.
* *title* - title of the page
* *uniqueVisitsCount* - number of unique visits to this page
* *url* - url of the page 

</article>

<article id="2">

## Page Visited Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `pages-visited` pointing to the
collection of Pages Visited.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of pages visited
under the key `pages-visited`.

This is an example of a response to this url: http://localhost:8084/my-project/pages-visited/?page=0&size=1

```json
{
    "_embedded": {
        "pages-visited": [
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/pages-visited/AWOx3B_0TvifCU95Sg6d"
                    },
                    "interest": {
                        "href": "http://localhost:8084/my-project/interests/AWOx3B_0TvifCU95Sg6d"
                    },
                    "pages-visited": {
                        "href": "http://localhost:8084/my-project/pages-visited{?filter,page,size,sort*}"
                    }
                },
                "url": "https://www.liferay.com/page1",
                "description": "This is Page 1",
                "title": "Page 1",
                "ownerIdentifier": "AWOx3BczTvifCU95Sg6G",
                "uniqueVisitsCount": 2,
                "day": "2018-04-12T00:00:00Z",
                "identifier": "AWOx3B_0TvifCU95Sg6d",
                "ownerType": "individual",
                "interestName": "open source sharepoint alternative"
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8084/1527697379274/pages-visited/?page=0&size=20"
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

Creation of new Pages Visited or Update of Page Visited manually is not supported. Pages Visited are automatically
generated and updated from the Interest Chunks sent by the different Connectors.

Deletion of existing Pages Visited is not allowed for now either. 

Navigating through the list of pages visited, the link to each page visited can be found with the rel `self`. 

</article>