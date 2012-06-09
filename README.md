XmlValidate
===========

A quick & flexible XML validator capable of performing bulk validation of XML documents against XML Schemas as defined in the XML documents or forcing another schema target.

*XmlValidate* validates individual XML documents by filename or URL
or recursively searching directories with a list of target file
extensions. First documents are checked as being well-formed XML.
If parsing XML with non-validating parser fails then validation for that
file stops.

Here are three ways to validate XML documents using *XmlValidate*:

1. The *-map* option maps schema namespaces to given target schema locations.
It rewrites XML with location of schema instance document then validates against
target schema. If schema namespace is not found then it does not validate
the document. This validates the document against the actual schema namespace
defined in the document as opposed to the other approaches that rewrite
the XML documents to any target schema.

  old: &lt;kml xmlns="http://earth.google.com/kml/2.1">

  new: &lt;kml xmlns="http://earth.google.com/kml/2.1"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://earth.google.com/kml/2.1 file:/C:/xml/kml21.xsd"&gt;
 
Note if root element is "kml" and no namespace is specified then KML 2.0 namespace is used to validate document.
 
2. Specifing a schema without a namespace implies noNamespaceSchemaLocation attribute should be added to all XML documents and validate against that schema.
  no namespace schema:

     &lt;event xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:noNamespaceSchemaLocation="C:/cot/schema/Event.xsd"&gt;
 
3. And finally specify -ns and -schema options to provide a target namespace URI and schema location.  All XML documents specified in search location(s) will be rewritten and the target schemaLocation will be used regardless of the default namespace of that document.  So for example KML 2.1 documents can be validated against the 2.2 schema, and vice versa.
   old: &lt;kml xmlns="http://earth.google.com/kml/2.1"&gt;

  new: &lt;kml xmlns="http://www.opengis.net/kml/2.2"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.opengis.net/kml/2.2 file:/C:/xml/kml22.xsd"&gt;
 
Note that XML is reformatted so errors/warnings with line/column numbers are respect
to the reformatted content not the original so context is printed at that line/column
number after each error in order that errors can be tracked down and corrected in original XML document.
If you want the reformatted XML document printed then use -dump mode. If the error is in the XML Schema
not the instance document then the context will not be printed.

Note that XML is reformatted so errors/warnings with line/column numbers are respect to
the reformatted content not the original so context is printed at that line/column number
after each error in order that errors can be tracked down and corrected in original
XML document. If you want the reformatted XML document printed then use *-dump* mode.
If the error is in the XML Schema not the instance document then the context will not be printed.

##Usage
-----

Usage: XmlValidate [options] [-map=file] | [-schema=file (-ns=uri)] &lt;xml-document file, URL, or directory...&gt;
