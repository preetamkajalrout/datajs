# OData Code Snippets

This topic provides some snippets to help you get up and running quickly when using the datajs OData support.

**Reading and displaying data**
The following snippet shows how to read a list of category names from the Northwind service, using jQuery to add them to an element with ID 'target-element-id'. Because the service supports JSONP, the default network library uses it even though the document comes from a different domain.

```js
OData.read(
  "http://services.odata.org/Northwind/Northwind.svc/Categories",
  function (data) {
    var html = "";
    $.each(data.results, function (l) {
      html += "<div>" + l.CategoryName + "</div>";
    });
    $(html).appendTo($("#target-element-id"));
  }
);
```

**Reading paginated data**
The following snippet shows how to read a collection that the server sends down in pages, and concatenating all results into a single array.

```js
var arr = []();
var collectionUrl = "OData-url";

function readMoreData(url) {
  OData.read(url, function (data) {
    arr = arr.concat(data.results);
    if (data.__next) {
      readMoreData(data.__next);
    } else {
      alert("Done!");
    }
  });
}

readMoreData(collectionUrl);
```

**Adding data**
The following snippet shows how to add a new customer to an OData service that exposes a Customers resource set with Name, CustomerCategory and ID properties.

```js
OData.request(
  {
    requestUri: "/customer-service/Customers",
    method: "POST",
    data: { Name: "customer name", CustomerCategory: 123 },
  },
  function (insertedItem) {
    $("<div>inserted customer ID: " + insertedItem.ID + "</div>").appendTo(
      $("#target-element-id")
    );
  }
);
```

**Adding data in batches**
The following snippet shows how to add and cross-reference a Customer and an Order. Note that the first request includes a header for "Content-ID", and the second request references the newly added customer as "$1".

```js
// The request will be a batch of read and change operations.
// The changes all go together in a single **changeRequests.
var requestData = {
  __batchRequests: [
    {
      __changeRequests: [
        {
          requestUri: "Customers",
          method: "POST",
          headers: { "Content-ID": "1" },
          data: {
            CustomerID: 400,
            CustomerName: "John",
          },
        },
        {
          requestUri: "Orders",
          method: "POST",
          data: {
            OrderID: 400,
            Total: "99.99",
            Customer: { __metadata: { uri: "$1" } },
          },
        },
      ],
    },
  ],
};

OData.request(
  {
    requestUri: "/ScratchService.svc/$batch",
    method: "POST",
    data: requestData,
  },
  function (data) {
    // Look for errors within the responses.
    var errorsFound = false;
    for (var i = 0; i < data.__batchResponses.length; i++) {
      var batchResponse = data.__batchResponses[i](i);
      for (var j = 0; j < batchResponse.__changeResponses.length; j++) {
        var changeResponse = batchResponse.__changeResponses[j](j);
        if (changeResponse.message) {
          errorsFound = true;
        }
      }
    }

    // Display whether any errors were found, and a JSON-ified payload.
    alert(
      "Result (errors found=" +
        errorsFound +
        "):\r\n" +
        window.JSON.stringify(data)
    );
  },
  function (err) {
    alert(err.message);
  },
  OData.batchHandler
);
```

---

Previous Topic: [Using OData](./Using%20OData.md)
Next Topic: [OData Networking](./OData%20Networking.md)
