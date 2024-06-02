# Storing Preferences Sample

This sample demonstrates the use of the datajs Store API to store preferences for a web application.

datajs provides support for storing data in the browser such that it’s available even if the browser is restarted. The stores are used as key/value pairs, where the key is a string and the value is a Javascript value persisted through a JSON round-trip. In this sample, the store is used to persist color preferences for a web application.

This sample demonstrates:

- Creating a new store and opening an existing store
- Reading values from the store and handling error conditions
- Writing values to the store

The sample file is [store.htm](./Storing%20Preferences%20Sample_store.htm).

**Note:** The Store API does not work through file://, so the sample will not run if the HTML file is opened directly from the browser. Add the file to the [Local Service Sample](./Local%20Service%20Sample.md) project and run it from there.
