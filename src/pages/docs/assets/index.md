---
title: "Assets"
description: "Assets API"
layout: "guide"
icon: "hammer"
weight: 10
---

###### {$page.description}

<article id="1">

## The Asset Model

Assets contain information about documents, pages, forms or any content that the individual interacts with.

The entity asset contains the following fields:
* *identifier*
* *assetType*
* *dataSourceAssetPK*
* *dataSourceIdentifier*
* *name*
* *description*

</article>

<article id="2">

## Assets Collection

As described in [Initial Navigation to obtain URLs](/docs/general#navigation),
the `_links` section of the root resource will contain a template link labelled as `assets` pointing to the
collection of Assets.

This API supports [pagination](/docs/general#pagination), [sorting](/docs/general#sorting) and [filtering](/docs/general#filtering).

The response will contain inside the `_embedded` section, a list of assets
under the key `assets`.

This is an example of a response to this url: http://localhost:8084/my-project/assets?page=0&size=1

```json
{
    "_embedded": {
        "assets": [
            {
                "_links": {
                    "self": {
                        "href": "http://localhost:8084/my-project/assets/AWNoD0uDeO0_hdUWjxo_"
                    },
                    "assets": {
                        "href": "http://localhost:8084/my-project/assets{?filter,page,size,sort*}"
                    }
                },
                "name": "Liferay: Digital experience software tailored to your needs",
                "dataSourceAssetPK": "https://www.liferay.com/",
                "identifier": "AWNoD0uDeO0_hdUWjxo_",
                "dataSourceIdentifier": "AWNoD0HTeO0_hdUWjxoo",
                "assetType": "Layout"
            }
        ]
    },
    "_links": {
        "first": {
            "href": "http://localhost:8084/my-project/assets?page=0&size=1"
        },
        "self": {
            "href": "http://localhost:8084/my-project/assets?page=0&size=1"
        },
        "next": {
            "href": "http://localhost:8084/my-project/assets?page=1&size=1"
        },
        "last": {
            "href": "http://localhost:8084/my-project/assets?page=1&size=1"
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

Creation of new Assets or Update of Assets manually is not supported. Assets are automatically
generated and updated from the Activity Chunks sent by the different Connectors.

Deletion of existing Assets is not allowed for now either. 

Navigating through the list of assets, the link to each asset can be found with the rel `self`. 

</article>