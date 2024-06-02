# datajs store API

The store API provides a common abstraction over a few mechanisms that can be used to store data.

- In-memory
- DOM Storage
- IndexedDB Storage (experimental)

This store abstraction is essentially a key/value pair, where the key is a string and the value is a JavaScript value persisted through a JSON round-trip.

All of the store APIs work asynchronously. That is, you pass in success and error callbacks that fire sometime in the future, after the script has done its immediate work.

The success callbacks will return either the key/value pair as added or updated, or a true/false value indicating whether the key that was just operated on existed.

## datajs.createStore

```js
var store = datajs.createStore(name, mechanism);
```

- **_name_**: this is a string used to distinguish one store from another.
- **_mechanism_**: this is a string that can have one of the following values: "best", "memory", "dom" and "indexeddb".

If left unspecified, it `mechanism` defaults to `datajs.defaultStoreMechanism`, which in turn defaults to "best", which uses DOM if available and otherwise falls back to an in-memory store.

Once IndexedDB gains is stabilized and implemented in major browsers, "best" will use IndexedDB if available in preference of the other two mechanisms.

The createStore function immediately returns a `store` object.

## store.add

Adds a new key/value pair, fails if the key was already in the store.

```js
store.add(key, value, success /**(key, value)**/, error /**(error)**/);
```

## store.addOrUpdate

Adds a new key/value pair, updates the existing value if the key was already in the store.

```js
store.addOrUpdate(key, value, success /**(key, value)**/, error /**(error)**/);
```

## store.clear

Cleans up all data use and invalidates the store.

```js
store.clear(success /**()**/, error /**(error)**/);
```

## store.close

Closes any resources that the store may be using.

```js
store.close();
```

## store.contains

Checks whether the key is in the store.

```js
store.contains(key, success /**(bool)**/, error /**(error)**/);
```

## store.defaultError

This is a field with a function to be used when the error callback isn't specified; it defaults to throwing the error.

```js
store.defaultError = function (error) {};
```

## store.getAllKeys

Returns an array with all key values.

```js
store.getAllKeys(success /**(array)**/, error /**(error)**/);
```

## store.read

Gets a key/value back; fails if the key is not present.

```js
store.read(key, success /**(key, value)**/, error /**(error)**/);
```

## store.update

Updates the value for a key that is in the store; fails if the key is not in the store.

```js
store.update(key, value, success /**(key, value)**/, error /**(error)**/);
```

## store.remove

Removes a key and its value from the store if found; the success callback argument is true if the key was found, false otherwise.

```js
store.remove(key, success /**(bool)**/, error /**(error)**/);
```

---

Previous Topic: [OData Payload Formats](./OData%20Payload%20Formats.md)
Next Topic: [datajs cache API](./datajs%20cache%20API.md)
