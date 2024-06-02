# Simple CRUD Sample

Kudos to Lohith for posting [the first CRUD sample ever](http://kashyapas.com/2011/06/performing-crud-on-odata-service-using-datajs/) - thanks!

This sample builds on the [Local Service Sample](./Local%20Service%20Sample.md) and adds a page that can be used to create, read, update and delete ideas from the idea service.

Some techniques demonstrated include:

- How to craft requests that create, update and delete objects on ther service.
- Using [KnockoutJS](http://knockoutjs.com/) to simplify data binding, and using its mapping extension to turn service-friendly objects into UI-friendly objects and vice-versa.
- Adding a view model observable property, **communicating**, useful to avoid submitting multiple requests to the server at the same time.
- Embedded metadata to support round-tripping {{Date}} values when using JSON formatting.

The sample file is [ideas.htm](Simple CRUD Sample_ideas.htm), which can be added to the [Local Service Sample](./Local%20Service%20Sample.md) to run.

**Note**: When deleting ideas that have dependent information, the server will send back an error. The local service is set to use verbose errors which can lead to disclosing security information. The better practice is to have the service recognize this case and return an error that the client can the process appropriately, but because this requires writing code on both client and server, this sample doesn't demonstrate this and instead shows the full serialized error on the UI.
