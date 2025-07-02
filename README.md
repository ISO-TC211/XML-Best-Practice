# XML-Best-Practice
Work on best practice for XML schemas, validation and example for ISO/TC211<br/>
_ISO/TC 211 standards are model driven, being based on the UML model that defines packages, classes and attributes.<br/>
The XML implementations of those standards attempt to reproduce those models as schemas._<br/>
XML (eXtensible Markup Lanuage) provides a mechanism to transfer information and data in a structured way. The use of XML schemas further enhaces that tranfer by ensuring that the records that conform to that schema will tranfer the data in the order that the schema specifies and contains only the allowable values and always the mandatory ones. XML is very samble and is capable of transfering large pay-loads and dealing with very complex schemas.<br/>
Schemas that ISO/TC 211 promote are of two types, those that provide metadata about a set of data, and those that provide the data itself, and all conform to 'http://www.w3.org/2001/XMLSchema'.<br/>
In the past ISO/TC 211 has focused predominantly on the former, with the range of ISO 19115 metadata standards.
## Name spaces and their corresponding prefixes
For convenience, _name spaces_ will be expressed as 'nameSpace', and the associated _prefix_ as 'nsp'.<br/>
A nameSpace should be a URI for the concepts being defined by the classes and thier attributes.<br/>
For the majority of ISO/TC 211 XML implementations the nameSpace URI looks like a URL but need not be resolvable. The URIs generally reflect the URL where the XML schemas are located, but truncated after the major revision number. An example of this would, for the yet to be developed, ISO 19115-1 Edition 2 standard - Identifcation (nsp='mri'):<br/>
URI https:\/\/schemas.isotc211.org\/xml\/19115\/-1\/2\/mri\/1; and<br/>
URL https://schemas.isotc211.org/xml/19115/-1/2/mri/1.0/identification.xsd.<br/>
Except where the nameSpace has been adopted from a different domain (eg OGC) the nameSpace URI takes the form of:<br/>
_https:\/\/schemas.isotc211.org\/\<format\>\/\<standardNumber\>\/-\<partNumber\>\/\<EditionNumber\>\/\<nsp\>\/\<majorRevionNumber\>_; and<br/>
schema location takes the form of:<br/>
_https:\/\/schemas.isotc211.org\/\<format\>\/\<standardNumber\>\/-\<partNumber\>\/\<EditionNumber\>\/\<nsp\>\/\<majorRevionNumber\>.\<minorRevisionNumber\>\/\<fileName\>_<br/>
This pattern allows nameSpace URI to be maintained through minor revisions and patches, but change when major revisions occur. Using the model developed by <a href="https://semver.org/">Semantic Versioning 2.0.0.
## Structures used in metadata type schemas
ISO/TC 211 metadata type schemas represent each class in thge UML with three components: an introduction to the class as <complexType name="ClassName_PropertyType">; the class iytself as <element name="ClassName" type="nsp:ClassName_Type">; and the attributes of the class as <complexType name="ClassName_Type">


