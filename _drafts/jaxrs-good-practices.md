---
layout: post
title:  "JAX-RS good practices"
---

### REST API in Java

JAX-RS is a well known, formerly Java EE turned Jakarta specification for writing REST APIs in Java.
There are many ways to use it ; and, in my humble opinion, some are better than the others.

# - Use a marshalling provider

I've seen people directly call a Jackson's `ObjectMapper` in their method:
```java
ObjectMapper objectMapper = new ObjectMapper();
return objectMapper.writeValueAsString(myBean);
```

Instead, add a JAX-RS Provider and let it do its work: 
```xml
<dependency>
  <groupId>com.fasterxml.jackson.jaxrs</groupId>
  <artifactId>jackson-jaxrs-json-provider</artifactId>
  <version>2.12.3</version>
</dependency>
```
```java

return myBean;
```

It works for return types as well as input payload types.

## Use precise return types

Some people use the infamous `javax.ws.rs.core.Response` class as return types of all REST methods.
This is usually a bad idea, becauses it hides the true response type of the method.
```java
@GET
public Response getEntity() {
    return Response.ok().entity(theEntityBean).build();
}
```

As noted just before, you can just return the entity and it'll use a "200 Ok" response code and the provided marshaller.

```java
@GET
public EntityBean getEntity() {
    return theEntityBean;
}
```

A method returning void will usually respond with a "201 Created" response code.

If you need to manipulate HTTP response headers, 

## If you handle large data, don't load it fully in memory.

Use InputStream and StreamingOutputStream

## Split into interface and implementation

Some people put JAX-RS annotations directly on the business logic implementation.
```java
@POST
@Consumes(MediaType.APPLICATION_JSON)
public void writeEntity(Entity entity) {
```

You can then use the interface client-side too, thanks to a nifty trick from RESTEasy :
```java
build client proxy from interface
```

## OpenAPI / Swagger definition generation ?


