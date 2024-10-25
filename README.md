# Vapi Java Library
[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-SDK%20generated%20by%20Fern-brightgreen)](https://github.com/fern-api/fern)

The Vapi Java SDK provides convenient access to the Vapi API from Java or Kotlin.

## Installation

### Gradle

Add the dependency in your `build.gradle`:

```groovy
dependencies {
    implementation 'dev.vapi:server-sdk:0.x.x'
}
```

### Maven

Add the dependency in your `pom.xml`:

```xml
<dependency>
    <groupId>dev.vapi</groupId>
    <artifactId>server-sdk</artifactId>
    <version>0.x.x</version>
</dependency>
```

## Usage

Instantiate and use the client with the following:

```java
import com.vapi.api.Vapi;

Vapi vapi = Vapi.builder()
  .token("YOUR_TOKEN") 
  .build();

vapi.call.create();
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), a subclass of `VapiException`
will be thrown.

```java
import com.vapi.api.core.VapiException;

try {
    vapi.call.create();
} catch (VapiException e) {
    System.out.println(e.message());
}
```

The SDK also supports error handling for first class exceptions with strongly typed body fields. 

```java
import com.vapi.api.errors.InvalidRequestError;

try {
    vapi.call.create();
} catch (InvalidRequestError e) {
    System.out.println(e.message());
    System.out.println(e.getBody().getMissingField());
}
```

## Advanced

### Callbacks

You can register `callbacks` with the Vapi client and the callbacks will be invoked
after events have been sent or failed to send. 

```java
vapi.addCallback((event, statusCode) -> {
  if (statusCode >= 400) {
    // do something with event in error case    
  }    
});
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retriable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2).

A request is deemed retriable when any of the following HTTP status codes is returned:

- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) (Internal Server Errors)

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically.
Additions made directly to this library would have to be moved over to our generation code,
otherwise they would be overwritten upon the next generated release. Feel free to open a PR as
a proof of concept, but know that we will not be able to merge it as-is. We suggest opening
an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!
