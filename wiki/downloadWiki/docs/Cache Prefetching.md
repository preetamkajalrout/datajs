# Cache Prefetching

In many scenarios, users will view a large collection of data by being presented with some of the first items. If they're interested, they will typically move forward through the collection; in some cases, they will "jump ahead" quite a bit or decide to move all the way to the end.

The cache component of datajs provides built-in support for prefetching data to make this scenario faster. When a range of data is read with the `readRange` API, the cache will read that data as fast as possible, but will continue reading ahead a bit more of data. By the time the user scans the data and possibly decides to move forward again, it's likely that the data is already available in the cache and can be served without any additional round-trips.

There is no code that needs to be written to make this work - the cache component takes care of it automatically.

The prefetcher can be configured through the `options` object passed to the `cache`. If `prefetchSize` is defined, that will be the number of elements that are prefetched after any range is read. If left undefined, it will default to `pageSize`, so exactly one extra page of data is available.

Two special values can also be used:

- If `prefetchSize` is set to zero, the prefetcher is completely disabled.
- If `prefetchSize` is set to -1, the prefetcher will keep reading data until the end of the collection is found.

Setting `prefetchSize` to -1 and reading data starting at the beginning of the page is a useful way to read the whole collection into the cache, subject to any limits in the cache size or the underlying store.

```js
var cache = datajs.createDataCache({
  name: "mydata",
  source: "myurl",
  prefetchSize: -1,
});

cache.readRange(0, 10).then(function (data) { ... });

// At this point, the first ten items will be read,
// and the cache will keep working in the background,
// to help make all further reads faster.
```

---

Previous Topic: [Using Caches](Using%20Caches)
Next Topic: [Frequently Asked Questions](Frequently-Asked-Questions)
