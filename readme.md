uSync 
=

uSync is a Syncronization tool for the Umbraco CMS. It serailizes the database config and data
within an umbraco site, and reads and writes it to disk as a collection of xml files. 

uSync can be used as part of your source control, continious deployment or site syncronsation plans. 

Out of the box, uSync reads and writes all the database elements to and from the `usync/data` folder.
It will save:

* Document Types
* DataTypes
* MediaTypes
* Templates
* Macros
* Languages
* Dictionary Items
* MemberTypes

You can use **uSync.ContentEdition** to manage content and media if you also want to write them to disk.

The basics workings of uSync
-
The main elements of uSync are the Serializers and Handlers:

### Serailizers
Serializers Mange the transtion between umbraco and the XML that uSync uses,
they control how the configuration is written in and out, manage things like 
internal IDs so your settings can move between umbraco installations. 

Serializerrs do the heavy lifiting of usync, and live in the uSync.Core package, 
you can use this package to programatically import and export data to umbraco. 

### Handlers
Handlers mange the storing of the XML and passing to the serializers. By default
this means reading and writing the xml to disk from the uSync folder. Handlers 
are the entry point for imports and exports, and they capture the save and delete
events inside of umbraco so that things are saved to disk when you make changes via
the back office. 

You can add your own handlers by implimenting the `ISyncHandler` interface.

### Mappers 
Mappers help with the content and media serialization process, they 
allow uSync to know how to find and map internal ids from within properties on your 
content. 
Within umbraco when you use links, and things like content pickers store the internal
id to link the property to the correct content. Between Umbraco installations these
ids can change so uSync needs to find them and map them to something more global (often GUIDS).

Mappers allow uSync to do this. as of v3.1 uSync.ContentEdition includes mappers for: 
* Built in editors *(RTE/Multinode treepicker, Content picker, etc)*
* The Grid
* Archetype
* Nested Content
* Vorto
* LeBlender
* DocTypeGrid

you can roll your own mappers, by implimenting the `IContentMapper` interface and putting 
settings in `uSyncCore.Config`

Packages
=
there are a number of uSync packages, that make up the uSync suite, most of the time
you don't need to worry about them, but they can be used in diffrent ways to give you
more control over how your data is handled.

uSync (BackOffice)
-
This is the main uSync package, it reads and writes the umbraco elements to disk. (uSync/data folder), 
It contains the Handlers and the main dashboard for uSync. 

uSync.Core
-
Core contains the serializers controlling the access to the umbraco system. Core allows you 
to pass and consume the xml representations of your umbraco site, it doesn't write anything to disk

uSync.ContentEdition
-
Content Edition is a set of additional handlers, to control Content and Media. 

uSync.Snapshots
-
Snapshots is a diffrent approach to saving the umbraco settings. the standard uSync saves all changes
as they are made into the standard uSync folder. 

Snapshots allows you to capture a series of changes into a single snapshot, which can then be moved around
either as part of a wider collection or as a single change. 


