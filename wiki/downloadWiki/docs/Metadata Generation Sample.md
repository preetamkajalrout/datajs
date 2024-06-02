# Metadata Generation Sample

This sample demonstrates how to use the metadata handler to read metadata from an OData service.

While it's possible to do this at runtime, this incurs an extra round-trip to the server, usually on the path of first display of the page. The recommended practice is to get the metadata at development time and embed it along with the script used to handle the service.

The [metadata.htm](./Metadata%20Generation%20Sample_metadata.htm) file provided reads metadata from the [Local Service Sample](./Local%20Service%20Sample.md) and displays code that can be added to your script to initialize the metadata on script load, without an additional round-trip to the server.

To adapt this sample, simply set the metadata URL to a different value and copy the page to the web server where the data service is hosted to avoid cross-domain access problems.
