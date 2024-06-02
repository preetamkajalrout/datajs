# Local Service Sample

This sample sets up a local OData service that is used by some other datajs samples.

The sample unpacks into a Microsoft Visual Studio project with the following interesting components.

# A database script, `ideas.sql`. You can run this from a command prompt window with `sqlcmd -S .\SQLEXPRESS -E -e -i ideas.sql` to set up a simple database with sample data on a local Microsoft SQL Express service.

# An ADO.NET Entity Framework model in an empty ASP.NET web project that maps the database into an object model.

# A WCF Data Service that exposes this model using OData in that same project.

# Various javascript files to simplify getting other samples.

This makes running other [Samples](./Samples.md) as simple as adding a new HTML file to this project and running it.

[SimpleService.zip](./Local%20Service%20Sample_SimpleService.zip)
