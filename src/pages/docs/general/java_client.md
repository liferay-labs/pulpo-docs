---
title: "Java Client"
description: "You can use this Java client to consume the API from Java Code."
layout: "guide"
weight: 4
---

###### {$page.description}

Pulpo provides a Java Client that allows you to consume this API inspired by the [Traverson JavaScript library](https://blog.codecentric.de/en/2013/11/traverson/)

This client will help you navigate the API leveraging its hypermedia capabilities.

<article id="setup">

## Set Up

In order to use the Client, you should have this gradle dependency:

```
compileInclude group: "com.liferay.osb.pulpo", name: "com.liferay.osb.pulpo.engine.contacts.client", version: "0.0.1-20180312.115409-14"
```

In case you need also the transitive dependencies, you should add:

```
compileInclude group: "com.eclipsesource.minimal-json", name: "minimal-json", version: "0.9.4"
compileInclude group: "com.fasterxml.jackson.core", name: "jackson-annotations", version: "2.6.3"
compileInclude group: "com.fasterxml.jackson.core", name: "jackson-core", version: "2.6.3"
compileInclude group: "com.fasterxml.jackson.core", name: "jackson-databind", version: "2.6.3"
compileInclude group: "com.github.javafaker", name: "javafaker", version: "0.13"
compileInclude group: "com.github.mifmif", name: "generex", version: "1.0.2"
compileInclude group: "com.github.wnameless", name: "json-flattener", version: "0.4.1"
compileInclude group: "com.liferay", name: "com.liferay.petra.lang", version: "1.1.2"
compileInclude group: "dk.brics.automaton", name: "automaton", version: "1.11.2"
compileInclude group: "org.apache.commons", name: "commons-lang3", version: "3.5"
```

If you are using the client from an OSGI runtime, you may need to exclude some
packages. See [one example](https://github.com/liferay/com-liferay-pulpo-connector-de-private/blob/7.0.x-private/pulpo-connector-de-contacts-demo/bnd.bnd).

</article>

<article id="obtainClient">

## Obtain the client

You can obtain a Contacts Client instance using the ContactsClientFactory by pointing it to an [engine URL](/#environments) and a ProjectID. 

```
ContactsEngineClient contactsEngineClient = 
	ContactsEngineClientFactory.getClient(
		"https://contacts-dev.liferay.com/" "MY-PROJECT-ID");
```

You then go ahead and define the relation names you want to discover and follow. relation names can either be simple names or [JSONPath](http://goessner.net/articles/JsonPath/) expressions (starting with an $).

</article>

<article id="traversing">

## Traversing the API

Now, we will show some examples of how to traverse the API using the client.

For example, to obtain the list of data sources, you could do:

```
PagedResources<DataSource> pagedResources =
	contactsEngineClient.follow(
		ContactsEngineClient.DATA_SOURCES
	).withTemplateParameters(
		contactsEngineClient.getDefaultTemplateParameters()
	).toObject(
		new TypeReferences.PagedResourcesType<DataSource>() {
		}
	);
			
Collection<DataSource> dataSources = pagedResources.getContent();
```

Another example, to obtain just one datasource by the identifier:

```
Map<String, Object> parameters = contactsEngineClient.getDefaultTemplateParameters();

parameters.put(ContactsEngineClient.IDENTIFIER, "My-DataSource-Identifier")

DataSource dataSource = contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).toObject(
	DataSource.class
);
```

Or one more, just to obtain the name of the datasource:

```
Map<String, Object> parameters = contactsEngineClient.getDefaultTemplateParameters();

parameters.put(ContactsEngineClient.IDENTIFIER, "My-DataSource-Identifier")

String dataSourceName = contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).toObject(
	"$.name"
);
```

These samples hand a parameter map into the execution (withTemplateParameters). 
The parameters will be used to expand URIs found during the traversal that are templated (the projectId variable is already filled when you call `contactsEngineClient.getDefaultTemplateParameters()`). 

In the case of the last example, we evaluate a JSONPath expression to access the data source’s name.

The examples listed above show a simple version of traversal with just one hop. A more complex example with several hops could look like this:

```
Map<String, Object> parameters = contactsEngineClient.getDefaultTemplateParameters();

parameters.put(ContactsEngineClient.IDENTIFIER, "My-Individual-Identifier")

String individualSegmentName = contactsEngineClient.follow(
	ContactsEngineClient.INDIVIDUAL, ContactsEngineClient.INDIVIDUAL_SEGMENTS   
).withTemplateParameters(
	parameters
).toObject(
	"$.name"
);
```

In this previous example, the follow method received 2 rels that will trigger 2 hops. At each hop, the same template parameters are applied. However, it could be customized at each hop:

```
String individualSegmentName = 
	contactsEngineClient
		.follow(
			Hop.rel(ContactsEngineClient.INDIVIDUAL).withParameter("identifier", "12345"))
		).follow(
			ContactsEngineClient.INDIVIDUAL_SEGMENTS
		).follow(
			"$._embedded.individual-segments[0]"	
		).withTemplateParameters(
			commonParameters
		).toObject(
			"$.name"
		);
```

The `Hop.rel(String rel​)` function is a convenient way to create a single Hop. Using .withParameter(key, value) makes it simple to specify URI Template variables for just one hop. You can chain as many .withParameter() as needed or even pass a Map using .withParameter(Map).

The follow() method is chainable, meaning you can string together multiple hops as shown above. You can either put multiple, simple string-based rels (follow("individuals", "individual-segments")) or a single hop with specific parameters.

</article>

<article id="return">


## Return Objects

There are different types of objects you can retrieve from the API, and you can decide which one you want.

For example, if you want to obtain just one object (DataSource, Individual...) you just need to specify it using the .toObject method.

```
DataSource dataSource = contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).toObject(
	DataSource.class
);
```

In some situations you may not only want the object, but the full "Resource" which contains the object and its links, then you can pass a new TypeReference to the toObject method:

```
Resource<DataSource> dataSourceResource =
contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).toObject(
	new TypeReferences.ResourceType<DataSource>() {
	}
);

List<Link> links = dataSourceResource.getLinks();

String selfHref = dataSourceResource.getLink("self").getHref();

DataSource dataSource = dataSourceResource.getContent();
```

You can also obtain more information about the HTTP Request doing toEntity. It will return the `ResponseEntity<T>` of the type you especified.

```
ResponseEntity<DataSource> dataSourceResponseEntity = contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCES
).withTemplateParameters(
	contactsEngineClient.getDefaultTemplateParameters()
).toEntity(
	DataSource.class
);

Assert.assertEquals(HttpStatus.OK, dataSourceResponseEntity.getStatusCode());

DataSource dataSource = dataSourceResponseEntity.getBody();
```

When working with collections, the server will return "pages" of entities which will help you paginate the collection. In this situation the you have similar options:

if you want to obtain the list of objects (DataSource, Individual...) you just need to specify it passing an instance of `TypeReferences.PagedResourcesType<DataSource>` to the .toObject method.

```
PagedResources<DataSource> pagedResources =
	_contactsEngineClient.follow(
		ContactsEngineClient.DATA_SOURCES
	).withTemplateParameters(
		parameters
	).toObject(
		new TypeReferences.PagedResourcesType<DataSource>() {
		}
	)
```

The PagedResources object will contain the "Content" with the DataSources, the "metadata" with information about the pagination and the "links" with links to other resources.
For example:

```
// Content

Collection<DataSource> dataSources = pagedResources.getContent();

// MetaData

PagedResources.PageMetadata metadata = pagedResources.getMetadata();

long totalElements = metadata.getTotalElements();
int pageSize = metadata.getSize();

// Links

List<Link> links = pagedResources.getLinks();

String nextHref = pagedResources.getLink("next").getHref();
``` 

When you want to obtain the "page" of "Resources" which contains the objects and its links, then you can pass an instance of `TypeReferences.PagedResourcesType<Resource<DataSource>>` to the toObject method:
```
PagedResources<Resource<DataSource>> pagedResources =
	_contactsEngineClient.follow(
		ContactsEngineClient.DATA_SOURCES
	).withTemplateParameters(
		parameters
	).toObject(
		new TypeReferences.PagedResourcesType<Resource<DataSource>>() {}
	)
	
// Content

Collection<Resource<DataSource>> dataSources = pagedResources.getContent()
```

In this last example, the type of the Content was different, but the Metadata and Links were the same.
</article>

<article id="postputdelete">

## Post, Put and Delete

You can also use the client to create, update or delete entities. You first need to follow the relationships until the desired endpoint and then execute your method.
For example, this would create a datasource:

```
ResponseEntity<DataSource> dataSourceResponseEntity = contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCES
).withTemplateParameters(
	contactsEngineClient.getDefaultTemplateParameters()
).post(
	_getSampleDataSource("MY-DATASOURCE"), MediaType.APPLICATION_JSON
).toEntity(
	DataSource.class
)

Assert.assertEquals(
	HttpStatus.OK, dataSourceResponseEntity.getStatusCode())

DataSource createdDataSource = dataSourceResponseEntity.getBody()	
```

This would update a DataSource:

```
Map<String, Object> parameters =
	contactsEngineClient.getDefaultTemplateParameters()

parameters.put(IDENTIFIER, identifier)

contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).put(
	dataSource, MediaType.APPLICATION_JSON
).toEntity(
	DataSource.class
)
```

And this would delete a DataSource:

```
Map<String, Object> parameters = contactsEngineClient.getDefaultTemplateParameters()

parameters.put(IDENTIFIER, identifier)

contactsEngineClient.follow(
	ContactsEngineClient.DATA_SOURCE
).withTemplateParameters(
	parameters
).delete(
).toEntity(
	DataSource.class
)
```

</article>

* All these examples can be found in [github](https://github.com/liferay/com-liferay-osb-pulpo-engine-contacts-private/tree/7.0.x-private/osb-pulpo-engine-contacts-client-functional-test/src/testFunctional/groovy/com/liferay/osb/pulpo/engine/contacts/client/functional/test).





