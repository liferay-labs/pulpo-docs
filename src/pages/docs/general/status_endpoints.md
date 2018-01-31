---
title: "Status Endpoints"
description: "The following endpoints can be queried to obtain information about the status of the engine."
layout: "guide"
weight: 3
---

###### {$page.description}

<article id="health">

## API Health

The health of the API can be checked at the endpoint: `/management/health`

The response will be JSON in this format with posible statuses UP and DOWN:

```
{
	"description":"Pulpo Contacts Engine",
	"status":"UP"
}
```
</article>

<article id="version">

## API Deployed Version

The version of the API that is running can be checked at the endpoint: `/management/info`

The response will be JSON in this format, with information relative to the branch deployed, the time it was built, the version of the API and the git commit:

```
{
	"branch":"PULPO-166.hateoas",
	"buildtime":"20180103T142812Z",
	"version":"1.0.0",
	"revision":"e015458b74a1378801c26d9eb5756c5a245d885d"
}
```
</article>

