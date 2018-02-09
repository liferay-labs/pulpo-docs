---
title: "Authentication"
description: "Any request to this API will have to be signed, otherwise you will receive a 401 error response."
layout: "guide"
weight: 2
---

###### {$page.description}

<article id="intro">

## Authentication Headers

In order to have a signed request, it must contain the following http headers:
- *timestamp*: the timestamp of the request in milliseconds
- *apikey*: your client key for accessing this API (ask for one to the Pulpo Team)
- *signature*: your request signed. In order to sign your request, you can use the SignatureGenerator utility to obtain this signature.
</article>

<article id="signature">

## Signature Generation

You can use the following gradle dependency to use the `SignatureValidator.java` utility:
    
```
provided group: "com.liferay.osb.pulpo", name: "com.liferay.osb.pulpo.engine.contacts.security.signature", version: "0.0.1-20180508.134750-15"
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

*1) Include the api key Header in your request
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
