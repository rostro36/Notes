# Big Data Week 04 readings
## [JSON Standard](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)
### General
JSON is a text syntax that facilitates structured data interchange and depends on Unicode in the hex numbers used in the \u escapement notation, this means JSON is programming language independent and the standard only defines conformance, not how to interpret the text e.g. if an object is a list or an array.

JSON does not support cyclic graphs, also it is not optimized for binary data.

A JSON file consists of JSON text, which forms exactly one JSON value, which can be comprised of objects, arrays, numbers or strings. 
### JSON text
JSON text is formed out of strings, numbers and 9 tokens.

6 Structural tokens:
- [
- {
- ]
- }
- :
- ,

3 literal name tokens:
- true
- false
- null

White-spaces (tab, line feed, carry-return, space) are allowed before and after any token for prettifying, but not within any token, except strings.
### JSON values
A JSON value can be one of:
- object
- array 
- number 
- string
- literal name token
### Numbers
JSON is agnostic about the representation of numbers, they are saved as human readable.

![JSON numbers](../images/04_JSON_number.PNG)

Only one exponent allowed, only one leading 0 allowed, then dot or e. 

Non- numeric numbers like NaN or Infinity are not allowed.
### String
Unicode character wrapped in quotation marks. Escaping " \" ", "\\" with \ before, new line and friends are also with \n etc.. 

Any UTF-8 (in the basic multilingual plane) character can be represented by \u *(4-digit\*number)*, one can send non-UTF-8 chars (which have more than 4 digits) represented as a twelve-char sequence encoding in the UTF-16 surrogate pair, but if it is seen as two UTF-8 or one UTF-16 is the job of the processor, not the language.
### Objects
An object is represented as (*name*/*value*)\* pairs surrounded by a pair of curly bracket tokens. Where a *name* is a string, followed by a single colon, to separate it from the *value*.

A single comma separates *value* from the next name.

JSON names can be any string, doubled and the ordering is not guaranteed, like dicts in Python.
### Arrays
Place *values* inside of square brackets and separate one *value* from the other by a comma.

The ordering is again open.
## [XML Fundamentals](https://docstore.mik.ua/orelly/xml/xmlnut/ch02_01.htm)
### General
XML contains text, never binary data.

XML filenames do not say anything about the content.
### Element
<*tag*> *content*</*tag*> defines an element.

<*tag*/> acts exactly the same as an empty element without content.

Tags are case-sensitive.
### Content tree
Content is a tree of elements or a value in itself. The different children don't have to be separated by anything, but they must be in the correct order; you have to close a child in the reverse order as you have opened them.

There is exactly one root element, so you can not have two "children" at the top level.
### Attributes
Attributes can be values declared in the tag e.g. *<name first="Alan" last="Turing"/>*, you can use " or '. The order is irrelevant, but therefore each tag can only one attribute with the same name.
### XML names
XML element names and XML attribute names are bundled in the term XML names, which allow pretty much any alphanumeric character plus special chars like ü, ç, from punctuation only "-","\_" and "." are allowed. But the colon is reserved for namespaces.
White-spaces are disallowed. XML names have to start with letters, ideograms(ö,ç etc.) or "\_".
### Entity references
Content is not allowed to have a "<", you can escape it with "&lt;" the same for "&" and "\&amp;" other pre-defined entity references are:
- "\&gt;" for ">"
- "\&quot;" for "
- "\&apos;" for '
### CDATA sections
You can pass literal text using the starting tag "<\!\[CDATA\[\" and the ending tag "]]>", so you do not need to escape characters. The only thing that is not possible to pass is the ending tag.
### Comments
Exist and are formed with "<\!--" and end with "-->", they should not include any other "--" and especially "--->" are disallowed for closing. As they are not part of the tree, they can be anywhere in the character data of a document, **not in tags**.
### Processing instructions
Processing Instructions are used to talk to a program, but do not hold actual data, that should be passed with the file. It is formed with "<\?*target*" and ended with "\?>", where *target* is the program that should read it, e.g. php.
The content of the instruction are target dependent and like comments can occur pretty much anywhere.
### XML declaration
The XML declaration looks like a processing instruction with target "xml" and attributes "version", "standalone" and "encoding" and if it exists, has to be placed at the beginning of the document, even before comments.

Standard encoding is UTF-8, but that can be changed in the declaration. The "standalone" attribute tells whether all the necessary information is in the document or it has to fetch some values from an external DTD, the default is "no", there is a DTD needed.

## [XML Namespaces](https://docstore.mik.ua/orelly/xml/xmlnut/ch04_01.htm)
### Purpose
- Distinguish between elements and attributes from different vocabularies with the same name.
- Group related names, so that the software can more easily read it.
### Syntax
Namespace assign names to URIs (*namespace names*). URIs often have special characters, they can not be in the XML like that and need a shorter prefix.
A name then is specified with:

*(prefix)*:*(name/local part)*=*qualified name*

Prefix and local part are made out of XML name characters except colons, the prefix xml is reserved to their dev team.

The prefix is not inherited from the parent and each child that needs this prefix has to append it again, but the scope where the prefix is known is inherited.
Prefixes can be redefined, where the "newest" is the strongest, but that is bad style.
```
<f:table xmlns:f="https://www.w3schools.com/furniture">
<f:name>African Coffee Table</f:name>
<f:width>80</f:width>
<f:length>120</f:length>
</f:table>
```
Prefixes are only variables, only the value behind (the URI) is important. The URIs should be explained, but can lead to 404, the standard does not define anything. The URIs are case-sensitive and do not check the page, only the name.
### Default namespace

```
<table xmlns="https://www.w3schools.com/furniture">
<name>African Coffee Table</name>
<width>80</width>
<length>120</length>
</table> 
```
The default namespace does not apply to elements with prefixes. Also default namespaces only work on elements, not on attributes.