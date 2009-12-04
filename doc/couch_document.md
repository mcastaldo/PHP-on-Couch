This section give details on using couchDocument data mapper

couchDocuments to simplify the code
===================================

CouchDB embed a simple JSON/REST HTTP API. You can simplify even more your PHP code using couch documents.
Couch Documents take care of revision numbers, and automatically propagate updates on database.

The basics
==========

Creating a new document
=======================

To create an empty couchDocument, simply instanciate the **couchDocument** class, passing the couchClient object as the constructor argument.

Example :

    $client = new couchClient('http://localhost:5984/','myDB');
    $doc = new couchDocument($client);

If I set a property on $doc, it'll be registered in the database. If the property is not _id, the unique identifier will be automatically created by CouchDB, and available in the couchDocument object.

Example :

    $doc->type="contact";
    echo $doc->id();
	// 1961f10823408cc9e1cccc145d35d10d

However if you specify _id, that one will of course be used.

Example :

    $doc = new couchDocument($client);
    $doc->_id = "some_doc";
    echo $doc->id();
    // some_doc

Setting properties
==================

Setting one property at a time
------------------------------

As we just saw, just set the property on the $doc object and it'll be recorded in the database

Example :

    $doc = new couchDocument($client);
    $doc->_id = "some_doc";
    $doc->type = "page";
    $doc->title = "Introduction";

Setting a bunch of properties
-----------------------------

It's always possible to set several properties in one query using the **set()** method

Example using an array :

    $doc = new couchDocument($client);
    $doc->set (
        array(
            '_id'   => 'some_doc',
            'type'  => "page",
            'title' => "Introduction"
        )
    );

Example using an object

    $prop = new stdClass();
    $prop->_id = "some_doc";
    $prop->type = "page";
    $prop->title = "Introduction";
    
    $doc = new couchDocument($client);
    $doc->set ( $prop );

Unsetting a property
--------------------

To unset a property, just use the **unset** PHP function, as you'll do for a PHP object.

Example :

    $prop = new stdClass();
    $prop->_id = "some_doc";
    $prop->type = "page";
    $prop->title = "Introduction";

    $doc = new couchDocument($client);
    $doc->set ( $prop );
    unset($doc->title);
    echo $doc->title ; // won't echo anything

Fetching a couchDocument from the database
==========================================

The static method **getInstance()** returns a couchDocument when the specified id exists :

Example :

    $doc = couchDocument::getInstance($client,'some_doc');
    echo $doc->_rev."\n";
    echo $doc->type;

Getting back classic PHP object
===============================

To get the couch document fields from a couchDocument object, use the **getFields()** method


Example :

    $doc = couchDocument::getInstance($client,'some_doc');
    print_r($doc->getFields());
    /*
        stdClass object {
            "_id"  => "some_doc",
            "_rev" => "3-234234255677684536",
            "type" => "page",
            "title"=> "Introduction"
        }
    */
