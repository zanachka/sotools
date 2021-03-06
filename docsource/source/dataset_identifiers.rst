Identifiers for Datasets
========================

.. contents:: Contents
   :local:

Overview
--------

The purpose of this document is to describe the behavior of DataONE indexers when encountering
identifiers in ``SO:Dataset`` [#dataset]_ instances.

In the context of DataONE, a dataset has multiple components. Each component version is preserved
and each component version has a persistent, globally unique identifier (PID). Each component may
also be assigned a globally unique identifier that always resolves to the most recent version
of a component (SeriesID or SID). That context is used in this document.

.. graphviz::

   digraph foo {
      rankdir=LR;
      graph [fontname="avenir", fontsize=10];
      node [fontname="avenir", fontsize=10];
      edge [fontname="avenir", fontsize=10];
      Dataset [shape=rectangle, style=filled, fillcolor=darkseagreen2];
      PropertyValue [shape=rectangle, style=filled, fillcolor=darkseagreen2];
      Text [shape=ellipse, style=filled, fillcolor=lightskyblue2];
      URL [shape=ellipse, style=filled, fillcolor=lightskyblue2];
      Text2 [label=Text, shape=ellipse, style=filled, fillcolor=lightskyblue2];
      URL2 [label=URL, shape=ellipse, style=filled, fillcolor=lightskyblue2];
      DOI [label="\"DOI\"", shape=ellipse, style=filled, fillcolor=lightskyblue2];
      dcdoi [label="datacite:doi", shape=parallelogram, style=filled, fillcolor=khaki];
      dcri [label="datacite:ResourceIdentifier", shape=rectangle, style=filled, fillcolor=goldenrod1];
      Dataset -> PropertyValue [label=identifier];
      Dataset -> URL [label=identifier];
      Dataset -> Text [label=identifier];
      PropertyValue -> Text2 [label=value];
      PropertyValue -> URL2 [label=url];
      PropertyValue -> DOI [label=propertyID];
      PropertyValue -> dcdoi [label="datacite:usesIdentifierScheme"]
      PropertyValue -> dcri [arrowtail=normal, dir=both, style=dotted]
   }

id and identifier
-----------------

The ``@id`` property in JSON-LD [#id]_ identifies a node in the RDF graph, and must be an IRI [#IRI]_.
The ``SO:identifier`` is an optional property of a node that may or may not be a URI, and may or may
not be the same as the ``@id`` for the node.



Ideally, the ``@id`` and the ``SO:identifier`` would
have the same value though this if often not the case for datasets.

Identifier Conflation
---------------------

The string "978-1-5387-1847-6" is an identifier, in this case an ISBN. A number 
of services are available to provide more information about the subject of the 
identifier. For example, `ISBN Search`_ is a lookup service that provides a HTML 
view of the results. Goole provides a `Books API`_ that returns structured data,
though requires anuthentication to use, for example::

  curl "https://www.googleapis.com/books/v1/volumes?key=${GAPIKEY}&q=isbn:9781538718476"

  {
    "kind": "books#volumes",
    "totalItems": 1,
    "items": [
     {
      "kind": "books#volume",
      "id": "SyqzDwAAQBAJ",
      "etag": "q7NUsBTwiu8",
      "selfLink": "https://www.googleapis.com/books/v1/volumes/SyqzDwAAQBAJ",
  ...

  Note that the canonical form of the identifier is "``9781538718476``", the commonly
  used human readable form is "``978-1-5387-1847-6``", and a resolvable form that 
  varies with the resolving service such as the aforementioned Google Books API.


.. ISBN Search: https://isbnsearch.org/isbn/9781538718469
.. Books API: 

Persistence
-----------

There is no notion of immutability in schema.org.

Foototes
--------

.. [#id] IRIs are a fundamental concept of Linked Data, for nodes to be truly linked, dereferencing
         the identifier should result in a representation of that node.
         https://www.w3.org/TR/json-ld/#node-identifiers
.. [#IRI] An IRI (Internationalized Resource Identifier) is a string that conforms to the
          syntax defined in :rfc:`3987`
.. [#dataset] https://schema.org/Dataset
.. [#identifier] http://schema.org/docs/datamodel.html#identifierBg


.. include:: includes/binder_activate.rst
