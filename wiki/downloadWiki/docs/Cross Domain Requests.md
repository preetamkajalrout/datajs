# Cross Domain Requests

Browsers have a policy commonly referred to as the [same origin policy](http://en.wikipedia.org/wiki/Same_origin_policy) that blocks requests across domain boundaries. Because of this restriction update operations cannot be performed if the web page is served by one domain and the target OData endpoint is in a different one. Users have the ability to disable this policy in the browser, however it is typically turned on by default. datajs is designed with such an assumption. The following options are available to support this scenario:

- Have the web server provide a relaying mechanism to redirect requests to the appropriate OData endpoint.
- Use an [XDomainRequest](<http://msdn.microsoft.com/en-us/library/cc288060(VS.85).aspx>) object. This option is not available in all browsers.
- Use cross origin `XMLHttpRequest` object. This option is also not available in all browsers.
- Prompt for consent on first use. This option is not available in all browsers and generally provides a poor user experience.

Read operations are available for cross-domain requests if the OData server supports JSONP. To configure the datajs to use JSONP, set the following properties on the `OData.defaultHttpClient` object.

- `formatQueryString`: this is a query string option inserted in the URL.
- `callbackParameterName`: this is the name of a query string option that will be assigned a function name to call back into.
- `enableJsonpCallback`: see [OData Security](./OData%20Security.md) before setting this property to `true`.

By default, all values except for `enableJsonpCallback` are set to work with the popular [JSONP extension](http://code.msdn.microsoft.com/DataServicesJSONP) for WCF Data Services-based servers, but can be modified to any appropriate value before a request is submitted.

## The values can also be set on an individual `request` object when passed to the `OData.request` API.

Previous Topic: [OData Security](./OData%20Security.md)
Next Topic: [Using Stores](./Using%20Stores.md)
