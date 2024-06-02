# OData Networking

The OData functionality in datajs is based on an **_httpClient_** object. The responsibility of the object is to implement an asynchronous network request through a **_request_** API.

By default, datajs will use the XMLHttpRequest object in the browser to make a network request. If JSONP is enabled, it will use this technique when the request is for a different domain.

You can use a different **_httpClient_** object by setting it on the appropriate API call or, in the case of the cache, by setting the **_httpClient_** property on the constructor options.

The default client is available through `OData.defaultHttpClient`, which is used when the caller doesn't specify one explicitly. You can use this directly as well, for example to implement a custom **_httpClient_** that modifies the request and then calls the original behavior. See [Custom OData httpClient](./Custom%20OData%20httpClient.md) for a sample.

Because the implementation is based on XMLHttpRequest, some of its constraints are also applicable to datajs. For example:

- Some headers are restricted and cannot be set, for example **_Referer_**.
- Redirections from the server have limited support. In some formats, relative URLs may be corrupted; in some browsers, the second request may change the headers - in particular, addressing the root of a service **_http://server/service_** may redirect to **_http://server/service/_** and cause the request to fail.
- [Cross Domain Requests](./Cross%20Domain%20Requests.md) have limited support.

---

Previous Topic: [OData Code Snippets](./OData%20Code%20Snippets.md)
Next Topic: [OData Security](./OData%20Security.md)
