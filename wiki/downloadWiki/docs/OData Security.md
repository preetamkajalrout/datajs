# OData Security

The default HTTP library works by using the XMLHttpRequest object, which in general doesn't allow the page to get data from a server different from the one that served the page (this is often referred to as the same origin policy.).

This means that the library generally trusts the server to provide non-malicious payloads (the library is, after all, also provided by the same server).

Note that JSONP effectively works by inviting a different server to run code in your page. At that point, your page could become compromised - that is, you would no longer be able to trust the values you have or the functions you call. This is why JSONP is disabled by default in the `OData.defaultHttpClient` object. If you trust the servers that you will be contacting, then you can turn support on by setting this property to true, as in the following snippet.

```js
OData.defaultHttpClient.enableJsonpCallback = true;
```

If you want to set this for a specific request instead of the library as a whole, you can pass this as an option in a request object.

```js
OData.request({
    requestUri: "http://www.example.org/jsonp-enabled-service/data",
    enableJsonpCallback: true
}, ... });
```

## To protect against man-in-the-middle attacks, you should enable and use the HTTPS protocol and refer to the data sources through 'https://' URLs.

Previous Topic: [OData Networking](./OData%20Networking.md)
Next Topic: [Cross Domain Requests](./Cross%20Domain%20Requests.md)
