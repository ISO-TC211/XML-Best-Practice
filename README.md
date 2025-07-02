# XML-Best-Practice
Work on best practice for XML schemas, validation and example for ISO\/TC&nbsp;211<br/>
_ISO\/TC&nbsp;211 standards are model driven, being based on the UML model that defines packages, classes and attributes.<br/>
The XML implementations of those standards attempt to reproduce those models as schemas._<br/>
XML (eXtensible Markup Lanuage) provides a mechanism to transfer information and data in a structured way. The use of XML schemas further enhaces that tranfer by ensuring that the records that conform to that schema will tranfer the data in the order that the schema specifies and contains only the allowable values and always the mandatory ones. XML is very samble and is capable of transfering large pay-loads and dealing with very complex schemas.<br/>
Schemas that ISO\/TC 211 promote are of two types, those that provide metadata about a set of data, and those that provide the data itself, and all conform to 'http://www.w3.org/2001/XMLSchema'.<br/>
In the past ISO\/TC 211 has focused predominantly on the former, with the range of ISO 19115 metadata standards.
## Namespaces and their corresponding prefixes
For convenience, _namespace prefix_ will be some times be 'nsp'.<br/>
A namespace should be a URI for the concepts being defined by the classes and thier attributes.<br/>
For the majority of ISO\/TC&nbsp;211 XML implementations the namespace URI looks like a URL but need not be resolvable. The URIs generally reflect the URL where the XML schemas are located, but truncated after the major revision number. An example of this would, for the yet to be developed, ISO&nbsp;19115-1&nbsp;Edition&nbsp;2 standard - Identifcation (nsp='mri'):<br/>
URI https:\/\/schemas.isotc211.org\/xml\/19115\/-1\/2\/mri\/1; and<br/>
URL https://schemas.isotc211.org/xml/19115/-1/2/mri/1.0/identification.xsd.<br/>
Except where the namespace has been adopted from a different domain (eg OGC) the nameSpace URI takes the form of:<br/>
_https:\/\/schemas.isotc211.org\/\<format\>\/\<standardNumber\>\/-\<partNumber\>\/\<EditionNumber\>\/\<nsp\>\/\<majorRevionNumber\>_; and<br/>
schema location takes the form of:<br/>
_https:\/\/schemas.isotc211.org\/\<format\>\/\<standardNumber\>\/-\<partNumber\>\/\<EditionNumber\>\/\<nsp\>\/\<majorRevionNumber\>.\<minorRevisionNumber\>\/\<fileName\>_<br/>
This pattern allows namespace URI to be maintained through minor revisions and patches, but change when major revisions occur. Using the model developed by <a href="https://semver.org/">Semantic&nbsp;Versioning&nbsp;2.0.0</a> which defines both minor and patch versions as backward compatable.<br/>
In ISO\/TC&nbsp;211 mame space prefixes should be three letters that reflect the name that the <a href="https://github.com/ISO-TC211/HMMG">Harmonised&nbsp;Model</a> has given the package or group of packages.
## Use of namespace prefixes within schemas
Always use namespace prefixes for all the namespaces in schemas. Whist it is possible to develop schemas that do not utilise namespace prefix for the target namespace, this is not good practice as the schema becomes more difficult to read. The exception to this is omission of the namespace prefix for _http:\/\/www.w3.org\/2001\/XMLSchema_, but even then the use of _xs_ or _xsi_ is recommended.<br/>
The inclusion of namespace prefixes does not slow down any processing of schemas. Noting that due to the complex interactions between most ISO\/TC&nbsp;211 schemas, rarely will only one namespace be present in a data file, so some prefixes are likely to be required. 
## Revision of schemas
ISO\/TC&nbsp;211 has a policy that all published valid schemas will be retained for as long as possible. As a result the versioning of XML schemas is critical. Using a modified construct as defined in <a href="https://semver.org/">Semantic Versioning 2.0.0</a> ISO\/TC&nbsp;211 increments the major version number in both the namespace URIs and schema URL when a change is made to a valid XML schema that is not backward compatable. For minor changes, where the revised schema is backward compatable with the pre-existing valid schema, the schema URL minor version number is incremented but the namespace URI is maintained. For patches and error correction, the revision numbers do not change but any changes that have been made are noted within the schema file as comments, explaining why a change has been made and what the content had been before the change. Note: changes to examples and validation files follow the same pattern but do not affect the nameSpace URIs.
## Structures used in metadata type schemas
ISO\/TC&nbsp;211 metadata type schemas represent each class in thge UML with three components: an introduction to the class as \<complexType name="ClassName_PropertyType"\>; the class iytself as \<element name="ClassName" type="nsp:ClassName_Type"\>; and the attributes of the class as \<complexType name="ClassName_Type"\><br/>
## Structures used in data type schemas
## Introduction of ISO 19103 Edition 2:2024
ISO&nbsp;19103&nbsp;Edition&nbsp;2:2024 has now replaced the withdrawn ISO&quot;19103:2015, with an expectation that all schemas generated by new projects will be consistent with ISO&nbsp;19103&nbsp;Edition&nbsp;2:2024.<br/>
Changes that have come about by the replacement of ISO&nbsp;19103:2015 by ISO&nbsp;19103&nbsp;Edition&nbsp;2:2024, have required that classifiers previously implemented with the namespace prefix “gco” need to be recoded and published. As those classifiers are used ubiquitously throughout the TC&nbsp;211 XML schemas and it will take some time for all schemas to be refreshed, there would be a high risk of namespace clashes if the new implementations were also to use “gco”. The new implementations use “cdt” (derived from package “Core Data Types” used in the Harmonised Model) xmlns:cdt=”https:\/\/schemas.isotc211.org\/xml/19103\/-\/2\/cdt\/1”.



