# datajs cache API

The datajs cache API provides a very simple way of reading large paginated results into the browser in an efficient way that is very responsive to the needs of the user interface.

For more information on using this API, see [Using Caches](./Using%20Caches.md).

## datajs.createDataCache

```js
var cache = datajs.createDataCache(options);
```

This returns a `cache` object. The cache object is created immediately, although it may delay-perform work.

The `options` object may have the following fields. The only required values are `name` and `source`.

- `name`: name of the cache (used by the underlying store)
- `source`: URI for the collection (without $top or $skip options)
- `cacheSize`: estimated maximum size of cached data in bytes.
- `pageSize`: page size for requests and prefetching
- `prefetchSize`: number of items to prefetch ahead of any request in a deferred manner, -1 to scan until the end of the collection, 0 to disable.
- `idle`: initial value for the `onidle` event.
- `mechanism`: if specified, indicates the store mechanism to use.
- {{metadata, httpClient, user, password, enableJsonpCallback, callbackParameterName, formatQueryString, httpClient}}: when the source is an OData URI, these values will be applied.

## cache.readRange

```js
var deferred = cache.readRange(startIndex, count);
```

This returns a deferred result. When the result completes, the success handler is invoked with the items starting from startIndex, up to a maximum of count. If the range is beyond the collection, the request still succeeds, but with zero items.

## cache.toObservable

```js
var observable = cache.toObservable();
```

This returns an Observable object if the [Reactive Extensions for Javascript](http://msdn.microsoft.com/en-us/data/gg577609) library is available. The observable will enumerate all the elements of the cache source, and can be used like other Observable objects for additional composition. The method is also available as `ToObservable`.

## cache.count

```js
var deferred = cache.count();
```

This returns a deferred result. When the result completes, the success handler is invoked with the count of items in the collection.

## cache.clear

```js
var deferred = cache.clear();
```

This returns a deferred result. When the result completes, all data in the cache will have been cleared, including supporting metadata if needed. Any pending operations will be canceled.

## cache.onidle

```js
cache.onidle = function () {};
```

This function will be called whenever the cache has finishes reading and prefetching work.

## deferred.then

```js
deferred.then(doneCallback, failCallback);
```

When invoked on a deferred result, this will register success and failure callbacks. This returns the specified deferred result.

## deferred.abort

```js
deferred.abort();
```

## When invoked on a deferred result, this will attempt to abort the operation. If the operation hasn't completed, the fail callback will be invoked, otherwise nothing will happen. This returns the specified deferred result.

Previous Topic: [datajs store API](./datajs%20store%20API.md)
