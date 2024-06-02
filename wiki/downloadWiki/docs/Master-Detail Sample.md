# Master-Detail Sample

This sample builds on the [Local Service Sample](Local-Service-Sample) and adds a page that can be used to add and remove comments from ideas in the idea service. In a typical master-detail scenario, a primary list is displayed (the "master" list), and as selection changes on this master list, another (the "details" list) is updated to display associated information.

Some techniques demonstrated include:

* How to load items from a collection on demand instead of eagerly and manage the collection locally.
* Using [Knockout](http://www.knockout.com/) to simplify data binding and tracking selection. 

A more advanced scenario where both master and detail elements can be further edited can be built by combining with techniques from the [Simple CRUD Sample](Simple-CRUD-Sample).

The sample file is [ideamd.htm](Master-Detail Sample_ideamd.htm), which can be added to the [Local Service Sample](Local-Service-Sample) to run.
