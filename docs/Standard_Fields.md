# Standard Fields

This section describes a set of standard message field definitions that should be used when similar concepts are needed. This will ensure the same concept has the same name and semantics across different APIs.

|Name|Type|Description|
|----|---|--------|
|name|	string|	The name field should contain the relative resource name.|
|offset|	int|	The pagination offset used in the execution of a collection query.|
|limit|	int|	The maximum number of items in a page used in the execution of a collection query.|
|createdAt|	timestamp|	The creation timestamp of a resource.|
|updatedAt|	timestamp|	The last update timestamp of a resource.|