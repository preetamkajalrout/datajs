# OData API

- [#OData.read](#odataread)
- [#OData.request](#odatarequest)
- [#Batch Operations](#batch-operations)
- [#OData.defaultSuccess](#odatadefaultsuccess)
- [#OData.defaultError](#odatadefaulterror)
- [#OData.defaultHttpClient](#odatadefaulthttpclient)

## OData.read

Reads data from the specified URL.

> OData.read = function (url | request, success(data, response), error(error), handler, httpClient, metadata)

- url - A string containing the URL to which the request is sent.
- request - An Object that represents the HTTP request to be sent.
- success(data, response) - A callback function that is executed if the request succeeds, taking the processed data and the server response.
- error(error) - A callback function that is executed if the request fails, taking an error object.
- handler - Handler object for the response data.
- httpClient - Object to use as an HTTP stack.
- metadata - Object describing the structural metadata to use.

This function is used to read data from an OData end point. In its simplest form this function takes a URL string to which an HTTP GET request is sent.

For example you can get all available Genres in the Netflix service by making the following call.

```js
OData.read("http://odata.netflix.com/v1/Catalog/Genres");
```

As part of the URI you can also pass in parameters.

```js
OData.read("http://odata.netflix.com/v1/Catalog/Genres?$top=3");
```

For full documentation on OData supported URIs refer to the [OData documentation](http://www.odata.org/developers/protocols/uri-conventions).

The `success` parameter of the `read` operation takes a callback function, which is executed if the request succeeds. If it is not defined then the default success handler is used from `OData.defaultSuccess`, which simply shows a dialog box with the results in a string form. It is a good practice to define your own success callback function.

```js
OData.read(
  "http://odata.netflix.com/v1/Catalog/Genres",
  function (data, request) {
    var html = "";
    for (var i = 0; i < data.results.length; i++) {
      html += "<div>" + data.results[i](i).Name + "</div>";
    }
    document.getElementById("target-element-id").innerHTML = html;
  }
);
```

The `read` function also takes an `error` parameter which is a callback function. The error callback is executed if the request fails. If the function is not defined then the default error handler `OData.defaultError` is invoked causing the error to be thrown as an exception.

```js
OData.read(
  "http://odata.netflix.com/v1/Catalog/Genres",
  function (data, request) {
    var html = "";
    for (var i = 0; i < data.results.length; i++) {
      html += "<div>" + data.results[i](i).Name + "</div>";
    }
    document.getElementById("target-element-id").innerHTML = html;
  },
  function (err) {
    alert("Error occurred " + err.message);
  }
);
```

A handler can optionally be passed to the `read` function. By default the MIME-based handler is used, which looks at the Content-Type header and uses the correct handler to parse the payload results. For example if the response's content type header is application/json then the JSON handler would be invoked to parse the data.

The `read` function also takes in an `httpClient` object. If it is not defined then the default httpClient object is used from `OData.defaultHttpClient`, which is set to a library that can take an HTTP request representation and return a response message from it.

The `metadata` parameter is optional, and defaults to OData.defaultMetadata. Handlers make use of metadata to enhance how they process data.

To allow finer control over what is sent in the request this function also allows passing in a request object instead of a simple URI string. The schema for this object is described in more detail under [#OData.request](#odatarequest).

The following example shows how to specify the accept type inside the request object, along with success and error callback functions.

```js
OData.read(
  {
    requestUri: "http://odata.netflix.com/v1/Catalog/Genres",
    headers: { Accept: "application/json" },
  },
  function (data, response) {
    alert("Operation succeeded.");
  },
  function (err) {
    alert("Error occurred " + err.message);
  }
);
```

The `read` operation returns a request value. The request value supports an `abort` invocation to cancel progress.

## OData.request

Sends a request containing OData payload to the server.

{{
OData.request = function (request, [success(data, response)](success(data,-response)), [error(error)](error(error)), [handler](handler), [httpClient](httpClient), [metadata](metadata))
}}

- request - An Object that represents the HTTP request to be sent
- success(data, response) - A callback function that is executed if the request succeeds, taking the processed data and the server response.
- error(error) - A callback function that is executed if the request fails, taking an error object.
- handler - Handler object for the request data.
- httpClient - Object to use as an HTTP stack.
- metadata - Object describing the structural metadata to use.

The `OData.request` function is a low level API for fine grained control over the request. It provides optional parameters to set callback functions, custom data handler and the HTTP client to use.

This API takes a request object as the first parameter that contains the headers, the target URI, the HTTP verb, which defines which CRUD operation to perform, and lastly the data on which the action takes place.

The request object should conform to the following signature:

```js
request = {
  headers: object, // object that contains HTTP headers as name value pairs
  requestUri: string, // OData endpoint URI
  method: string, // HTTP method (GET, POST, PUT, DELETE)
  data: object, // Payload of the request (in intermediate format)
  user: string, // (Optional) Username to send for BASIC authentication
  password: string, // (Optional) Password to send for BASIC authentication
};
```

The headers is an object containing key value pairs that let the user control the semantics of the HTTP request. For example user can specify the 'Accept' type for data or the version of the DataService. Whatever is defined in the headers object takes precedence over the defaults; however if a value is missing in the headers then the defaults are used. For example if the caller specifies a given DataServiceVersion, then handlers will not set the value for the DataServiceVersion header.

```js
var request = { headers: { DataServiceVersion: "2.0" } };
```

The requestUri is a string value that specifies the location on the OData endpoint to which the operation is targeted. For operations on existing items the URI should resolve to a single entity or link. For an insert operation this URI points to the resource set or collection to which the entity is inserted. For more information on OData operations refer to: http://www.odata.org/developers/protocols/operations

Here are few examples of how the requestUri would look for a given operation.

Add a new entity in an entity set
requestUri : http://services.odata.org/website/odata.svc/Customers

Add a new entity in a linked entity set
requestUri : http://services.odata.org/website/odata.svc/Customers(1)/Orders

Update an existing entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)

Update an existing linked entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)/Orders(1)

Merge an existing entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)

Merge an existing linked entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)/Orders(1)

Remove an existing entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)

Remove (i.e. set to null) a primitive property value in an existing entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)/FirstName/$value

Remove an existing linked entity
requestUri : http://services.odata.org/website/odata.svc/Customers(1)/Orders(1)

The method property in the request object determines, via an HTTP verb, the CRUD operation to perform. The following table shows the operations supported by the protocol and how they map to HTTP verbs.

- Add a new entity: POST
- Update an existing entity: PUT
- Update an existing entity with only the specified values: MERGE
- Delete an entity: DELETE

The data property defines the payload of the request defined in the intermediate format. The request is passed as input to the handler object which serializes the request payload to the appropriate wire format; as controlled by the request headers. The transformed data is then stored in the body property. Finally, the modified request object is passed to the underlying network stack.

Some update operations, such as POST, generate a response with the new item. This data is valuable because it provides useful metadata like the edit URI or concurrency information. It is also useful for any server generated property values. The response is processed by the appropriate handler. All responses trigger the invocation of either a success or error callback depending on the status of the HTTP response.

The success parameter of the request operation takes a callback function, which is executed if the request succeeds. If it is not defined then the default success handler is used from OData.defaultSuccess. It is a good practice to define your own success callback function.

The request function also takes an error parameter which is a callback function. The error callback function is executed if the request fails. If error callback function is not defined then the default error handler is used from the OData.defaultError.

Optionally a handler can be passed to the OData.request function. If it's not defined, the MIME-based handler is used, which looks at the Content-Type header and uses the correct handler to parse the payload results. For example if the data type is JSON then JSON handler would be invoked to parse the data.

The request function also takes in an httpClient object. If it is not defined then the default httpClient object is used from OData.defaultHttpClient.

## Batch Operations

datajs request API also supports OData batch operations that that allow grouping multiple operations into a single HTTP request. Following two examples show how to do batch operations for GET, PUT and POST by using **batchRequests and **changeRequests

```js
// Example 1:
OData.request(
  {
    requestUri: "http://ODataServer/FavoriteMovies.svc/$batch",
    method: "POST",
    data: {
      __batchRequests: [
        { requestUri: "BestMovies(0)", method: "GET" },
        { requestUri: "BestMovies(1)", method: "GET" },
      ],
    },
  },
  function (data, response) {
    //success handler
  },
  undefined,
  OData.batchHandler
);

// Example 2:
OData.request(
  {
    requestUri: "http://ODataServer/FavoriteMovies.svc/$batch",
    method: "POST",
    data: {
      __batchRequests: [
        {
          __changeRequests: [
            {
              requestUri: "BestMovies(0)",
              method: "PUT",
              data: { MovieTitle: "Up" },
            },
            {
              requestUri: "BestMovies",
              method: "POST",
              data: { ID: 2, MovieTitle: "Samurai" },
            },
          ],
        },
      ],
    },
  },
  function (data, response) {
    //success handler
  },
  undefined,
  OData.batchHandler
);
```

## OData.defaultSuccess

Default callback function for APIs that succeed.

> OData.defaultSuccess = function (data, response)

The OData.defaultSuccess property is used to fill in the value for calls that do not specify a success callback handler.

By default, it simply displays the string representation of data in an alert box. This is convenient for quick prototyping, for example you can type `(function(){OData.read("/myservice.svc/Customers");})()` in the location bar of the browser when datajs.js is available, and see the results in an alert box.

This property is not often used; instead, most API calls should include a situation-specific success handler.

## OData.defaultError

Default callback function for APIs that fail.

> OData.defaultError = function (error, response)

The OData.defaultError property is used to fill in the value for calls that do not specify an error callback handler.

By default, it simply throws the given error, which may go unhandled and break into the debugger if one is running in the browser. This is convenient for debugging, but typically the library consumer can do something more appropriate.

This property is often used as a generic error handler hook when all or most API calls end up routing through the same error handling mechanism.

Note that the error object may contain more or less information depending on when the error was found. For example, failures that occur when a request is being sent will not have a response property, but failures that occur while processing the response typically will have this property set.

## OData.defaultHttpClient

Provides the default HTTP layer for datajs.

> OData.defaultHttpClient = { request: function (request, success, error) }

The OData.defaultHttpClient property is used to fill in the value for calls that do not specify an HTTP client value.

The single property request provides a function that can be invoked with a request object, a success callback and an error callback.

By default, the library provides an HTTP client that relies on the XMLHttpRequest object to handle network requests.

The built-in HTTP client also provides properties to control the use of JSONP - see [Cross Domain Requests](./Cross%20Domain%20Requests.md) for additional information.

This property is typically left in its default state, although it may be replaced for advanced scenarios such as to add custom HTTP-level logging.

## For an example of how to create a custom HTTP client, see [Custom OData httpClient](./Custom%20OData%20httpClient.md).

Previous Topic: [References](./References.md)
Next Topic: [OData Payload Formats](./OData%20Payload%20Formats.md)
