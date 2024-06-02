# Frequently Asked Questions

This page provides some answers to frequently asked questions about datajs. If you have a question that is not covered here, try searching the [discussions area](../../../discussions/).

- [How can I access and use OData metadata through the datajs API?](#how-can-i-access-and-use-odata-metadata-through-the-datajs-api)
- [What is the relationship between JSON, OData, jQuery and datajs, and do I to choose between them?](#what-is-the-relationship-between-json-odata-jquery-and-datajs-and-do-i-to-choose-between-them)
- [How do I fix a "Unable to get value of the property 'parse': object is null or undefined" error?](#how-do-i-fix-an-invalid-character-or-jsonparse-error-error-when-reading-feed-data)
- [How do I fix a "window.JSON is null or not an object" error?](#how-do-i-fix-a-windowjson-is-null-or-not-an-object-error)
- [How do I fix an "Invalid Character" or "JSON.parse error" error when reading feed data?](#how-do-i-fix-an-invalid-character-or-jsonparse-error-error-when-reading-feed-data)
- [How do I use POST tunneling?](#how-do-i-use-post-tunneling)
- [How do I prevent a "long running script" error message from the browser?](#how-do-i-prevent-a-long-running-script-error-message-from-the-browser)

## How can I access and use OData metadata through the datajs API?

`OData.read` can be used to read and parse the metadata document from the service. datajs comes with a handler to do this, so it is not necessary for you to create a custom one; however it does not automatically choose it when parsing, so you need to specify it as the last argument to `OData.read` as in the following sample:

```js
OData.read(
  "<serviceRoot>/$metadata",
  function (metadata) {
    // success callback
  },
  function (error) {
    // error callback
  },
  OData.metadataHandler
);
```

On a successful response, the success callback will be called with the deserialized representation of the metadata passed in (the `metadata` variable in the example above). You can then pass this object into a subsequent `OData.read` call to instruct that call to use the metadata, as follows:

```js
OData.read(
  "<serviceRoot>/<endpoint>",
  function (data) {
    // success callback
  },
  function (error) {
    // error callback
  },
  undefined,
  undefined,
  metadata
);
```

Alternatively, if you are working with a single service and you'll be making multiple data reads that need to make use of the metadata, you can push the metadata onto the metadata stack:

```js
OData.defaultMetadata.push(metadata);
```

Then call `OData.read` on your data endpoints without needing to pass the metadata object into every call.

datajs does not perform any validation on the metadata document. It assumes the server sends the correct document.

## What is the relationship between JSON, OData, jQuery and datajs, and do I to choose between them?

Refer to [OData, jQuery and datajs](http://blogs.msdn.com/b/marcelolr/archive/2011/02/11/odata-jquery-and-datajs.aspx) for a detailed answer.

## How do I fix a "Unable to get value of the property 'parse': object is null or undefined" error?

This error is typically caused by a missing JSON object, and has the same cause as a ["window.JSON null" error](#how-do-i-fix-a-windowjson-is-null-or-not-an-object-error).

## How do I fix a "window.JSON is null or not an object" error?

IE6 and IE7 (along with later versions run in compatibility mode) do not have native JSON support so you will need to add an additional script reference to json2.js.

The following is a simple script tag that can be included in the document head to address this. Another solution is to include the file in your own web server and refer to it locally.

```html
<script
  type="text/javascript"
  src="http://cdnjs.cloudflare.com/ajax/libs/json2/20110223/json2.js"
></script>
```

## How do I fix an "Invalid Character" or "JSON.parse error" error when reading feed data?

Reading a feed or an entry from a service that uses an older version of WCF Data Services (formerly ADO.NET Data Services) might return an error if the payload data has single quotes (') in it. This is because WCF Data Services is escaping the single quotes before sending the data through the wire. To fix this issue it is necessary to install the [ADO.NET Data Services Update for .NET Framework 3.5 SP1](http://www.microsoft.com/downloads/en/details.aspx?displaylang=en&FamilyID=21f20103-551e-4501-89b3-e53fcac5cffd) on the server.

This issue doesn't impact services running on .NET Framework 4.0.

## How do I use POST tunneling?

POST tunneling allows you to send PUT, MERGE and DELETE requests when an HTTP processor won't let them through (including the browser in some cases). For more information, see [this blog post](http://blogs.msdn.com/b/marcelolr/archive/2008/08/20/post-tunneling-in-ado-net-data-services.aspx).

The following sample shows how to set up a request to use tunneling.

```js
var request = {
    ...
    method: "POST",
    headers: {
        ...
        "X-HTTP-Method": "MERGE"
    }
};
```

This request can then be used with the `OData.request` API. The important part is using "POST" as the method, and then using the intended method (PUT, MERGE or DELETE) in the X-HTTP-Method header.

## How do I prevent a "long running script" error message from the browser?

When doing a readRange from a cache with a large page size of a large collection the browser could throw an error stating that there is a “long running script” prompting the user to quit or continue. The exact size of the page at which this occurs is browser and collection dependent, but the library has been tested up to a 10MB page size with default browser settings. Reducing the page size of the cache prevents this error.

---

Previous Topic: [Cache Prefetching](./Cache%20Prefetching.md)
Next Topic: [References](./References.md)
