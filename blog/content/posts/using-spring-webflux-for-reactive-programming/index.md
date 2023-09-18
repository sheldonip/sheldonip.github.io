---
title: "Using Spring Webflux for reactive programming"
date: 2023-09-18T21:31:00Z
draft: false
image: cover.jpg
---

## Introduction

`WebClient` is included in spring webflux to handle HTTP requests. It is a non-blocking client for HTTP requests or streams based on Reactor. It is important for us to know the basic implementation of reactive programming.

### Implementation

To work with webflux and reactive streams, we need the following dependencies for a spring boot project:

- spring-boot-starter-webflux
- reactor-core
- reactor-netty

All of them can be found in maven central.

For publishing resources, it is just wrapping the response with `Mono` or `Flux`:

```java
@GetMapping("/{id}")
public Mono<Person> getPersonById(@PathVariable String id) {
    return personRepository.findPersonById(id);
}

@GetMapping
public Flux<Quote> getAllQuotes() {
    return quoteRepository.findAllQuotes();
}
```


To get an object from the body:

```java
WebClient client = WebClient.create("https://example.org");

Mono<Person> result = client.get()
		.uri("/persons/{id}", id).accept(MediaType.APPLICATION_JSON)
		.retrieve()
		.bodyToMono(Person.class);
```

To get a stream of objects:

```java
Flux<Quote> result = client.get()
		.uri("/quotes").accept(MediaType.TEXT_EVENT_STREAM)
		.retrieve()
		.bodyToFlux(Quote.class);
```

If the status code is `4xx` or `5xx`, `retrieve()` will throw `WebClientResponseException` . You may customise the handling by handlers:

```java
Mono<Person> result = client.get()
		.uri("/persons/{id}", id).accept(MediaType.APPLICATION_JSON)
		.retrieve()
		.onStatus(HttpStatus::is4xxClientError, response -> ...)
		.onStatus(HttpStatus::is5xxServerError, response -> ...)
		.bodyToMono(Person.class);
```

## Wrapping up
We have demonstrated a basic implementation for publishing resources and retrieving resources from a body or a stream. We also tried handled common HTTP status error codes upon retrieval. 

Now you have a basic understanding on using webflux for reactive programming.

## References

- https://docs.spring.io/spring-framework/reference/web/webflux-webclient.html
- https://www.baeldung.com/spring-webflux