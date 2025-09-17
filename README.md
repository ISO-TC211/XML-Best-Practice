# XML-Best-Practice

Work on best practice for XML schemas, validation and example for ISO/TC&nbsp;211.

> [!IMPORTANT]
> ISO/TC&nbsp;211 standards are model driven, being based on the UML model that defines packages, classes and attributes. The XML implementations of those standards attempt to reproduce those models as schemas.

XML (eXtensible Markup Language) provides a mechanism to transfer information and data in a structured way. The use of XML schemas further enhances that transfer by ensuring that the records that conform to that schema: 1) will transfer the data in the order that the schema specifies; and 2) contain only the allowable values and always the mandatory ones. XML is very samble and is capable of transfering large pay-loads and dealing with very complex schemas.

Note:
- XML is case sensitive but, except when entering values within tags, white-space agnostic.
- The implementation model will be slightly different from the conceptual model defined in the standard.
- Unlike UML, XML classes can only inherit (`substitutionGroup`) from one class. Where inheritance from more than one class is required, the attributes and roles from subsequent classes need to be instantiated in their own right within the specialized class.

Schemas that ISO/TC&nbsp;211 promote are of two types, those that provide metadata about a set of data, and those that provide the data itself, and all conform to 'http://www.w3.org/2001/XMLSchema'.

In the past ISO/TC&nbsp;211 has focused predominantly on the former, with the range of ISO 19115 metadata standards.

## Namespaces and their corresponding prefixes

For convenience, _namespace prefix_ will be some times be 'nsp'.

A namespace should be a URI for the concepts being defined by the classes and their attributes.

For the majority of ISO/TC&nbsp;211 XML implementations the namespace URI looks like a URL but need not be resolvable. The URIs generally reflect the URL where the XML schemas are located, but truncated after the major revision number. An example of this would, for the yet to be developed, ISO&nbsp;19115-1&nbsp;Edition&nbsp;2 standard - Identification (nsp='mri'):

- URI: `https://schemas.isotc211.org/xml/19115/-1/2/mri/1`
- URL: `https://schemas.isotc211.org/xml/19115/-1/2/mri/1.0/identification.xsd`

Except where the namespace has been adopted from a different domain (eg OGC) the namespace URI takes the form of:

- URI: `https://schemas.isotc211.org/<format>/<standardNumber>/-<partNumber>/<EditionNumber>/<nsp>/<majorRevionNumber>`
- URL: `https://schemas.isotc211.org/<format>/<standardNumber>/-<partNumber>/<EditionNumber>/<nsp>/<majorRevionNumber>.<minorRevisionNumber>/<fileName>`


This pattern allows the namespace URI to be maintained through minor revisions and patches, and to be changed when major revisions occur. That approach uses the [Semantic Versioning 2.0.0](https://semver.org/) model, which defines both minor and patch versions as backward compatible.

In ISO/TC&nbsp;211 namespace prefixes should be three letters that reflect the name that the [Harmonized Model](https://github.com/ISO-TC211/HMMG) has given the package or group of packages.

## Use of namespace prefixes within schemas

Always use namespace prefixes for all the namespaces in schemas. The target namespace in a schema defines the namespace that the root element of that schema belongs to. Whilst it is possible to develop schemas that do not utilise namespace prefix for the target namespace, this is not good practice as the schema becomes more difficult to read. The exception to this is omission of the namespace prefix for `http://www.w3.org/2001/XMLSchema`, but even then the use of `xs` or `xsi` is recommended.
The inclusion of namespace prefixes does not slow down any processing of schemas. Noting that due to the complex interactions between most ISO/TC&nbsp;211 schemas, rarely will only one namespace be present in a data file, so some prefixes are likely to be required. 

## Revision of schemas

ISO/TC&nbsp;211 has a policy that all published valid schemas will be retained for as long as possible. As a result the versioning of XML schemas is critical.

Using a modified construct as defined in [Semantic Versioning 2.0.0](https://semver.org/), ISO/TC&nbsp;211 increments the major version number in both the namespace URIs and schema URLs when a change is made to a valid XML schema that is not backward compatible.

For minor changes, where the revised schema is backward compatible with the pre-existing valid schema, the schema URL minor version number is incremented but the namespace URI is maintained.

For patches and error corrections, the revision numbers do not change, but any changes that have been made are noted within the schema file as comments, explaining why a change has been made and what the content had been before the change.

> [!NOTE]
> Changes to examples and validation files follow the same pattern but do not affect the namespace URIs.

## Structures used in metadata type schemas

ISO/TC&nbsp;211 metadata type schemas result in classes, attributes and roles being instantiated in the resultant XML records as tags. This is achieved by representing each class in the UML with three components:

1. an introduction to the class as `<xs:complexType name="ClassName_PropertyType">` (ElementPropertyType);
2. the class itself as `<xs:element name="ClassName" type="nsp:ClassName_Type">` (Element);
3. and the attributes of the class as `<xs:complexType name="ClassName_Type">` (ElementType).

The _Element_ is usually the name the Class has been given in the _Harmonised Model_. The ClassName should be consistent in all of these components.

The ElementPropertyType provides for a range of characteristics to be associated with the Element when expressed in the XML record. ElementPropertyType's use of `ref="nsp:ObjectReference` allows XML attributes that the Element can have in the XML record including `id` and `uuid` which are useful for linking internally and externally as anchors\/bookmarks. ElementPropertyType's use of `ref="nsp:nilReason` allows the XML elements defined in ElementType to carry XML attributes `nsp:nilReason="someText"` (explanation why a value has not been provided)

The Element defines if the class is a specialization of some other class, being expressed as XML attribute `substitutionGroup="nsp:GeneralisedElement"`, where the namespace prefix might not be the target namespace. The XML attribute `abstract="true"` identifies that that Element can not be instigated in its own right, but is the provider of elements for other specialisations of the class. `abstract="false"` is the default and need not be used. It should be noted that the GeneralizedElement in `substitutionGroup="nsp:GeneralizedElement"` must be matched with either `<xs:extension base="GeneralizedElement_Type">` or `<xs:restriction base="GeneralizedElement_Type">`.

The ElementType reflects the UML attributes and roles (element) associated with the class in the _Harmonized Model_ in the form:
`<xs:element name="element" type="nsp:Element_PropertyType">` where element should reflect the concept that the Element class.

The XML attributes that the `<xs:element>` must carry include `name="element"` or `ref="nsp:Element"`. In addition XML attributes `minOccurs="<Integer>"` and `maxOccurs="<Integer>"` where `<Integer>` should be provided to match the cardinality from the _Harmonized Model. The value provided in the XML attribute `type="nsp:Element_PropertyType"` must be either a `<xs:simpleType>` or `<xs:complexType>`.

If an Element is purely an implementation of a GeneralizedElement then the ElementType will be an `<extension base="nsp:GeneralizedElement_Type">` with a empty `<sequence>`.

An ElementType may also contain a `<any minOccurs="0" maxOccurs="unbounded" processContents="lax" namespace="##other"/>` but this needs to be used with extreme care as it is very likely that differing implementations will provide inconsistent responses to `<any>`. 

## Structures used in data type schemas

Unlike the metadata type schemas, ISO/TC&nbsp;211 data type schemas are simpler, using nested complexTypes, and W3C types directly for element values. These factors reduce the need to carry as many tags in the resultant XML record.

Although the data type schemas could use the `<complexType name="ClassName_PropertyType">`, they usually have `<element name="ClassName" type="ClassNameType>` followed by `<complexType name="ClassName_Type>` where the `<sequence>` contains: `<element name="element" type="<simpleType>"` or `<element ref="<nsp:ClassName>"`. The resultant XML record is then a set of tags for the ClassName containing tags for either other ClassNames, or elements with values. Similarly, where the `<sequence>` contains `<element name="element" type="<ClassName_Type>`, the resultant XML record is then a set of tags for the ClassName containing tags for elements in the referenced class. These options allow for greater compression in the XML data record by bypassing the UML role xor ClassName to avoid the extra XML tags.

Similar to metadata type schemas, data type scemas may also contain a `<any minOccurs="0" maxOccurs="unbounded" processContents="lax" namespace="##other"/>` but this needs to be used with extreme care as it is very likely that differing implementations will provide inconsistent responses to `<any>`.

## Introduction of ISO&nbsp;19103:2024

ISO&nbsp;19103:2024 has now replaced the withdrawn ISO&nbsp;19103:2015, with an expectation that all schemas generated by new projects will be consistent with ISO&nbsp;19103:2024.

Changes that have come about by the replacement of ISO&nbsp;19103:2015 by ISO&nbsp;19103:2024, have required that classifiers previously implemented with the namespace prefix `gco` need to be recoded and published. As those classifiers are used ubiquitously throughout the TC&nbsp;211 XML schemas and it will take some time for all schemas to be refreshed, there would be a high risk of namespace clashes if the new implementations were also to use `gco`. The new implementations use `cdt` (derived from package “Core Data Types” used in the Harmonised Model) `xmlns:cdt="https://schemas.isotc211.org/xml/19103/-/2/cdt/1"`.



