# Big Data Week 04

## Usage
- Why should we have a unified syntax?
	- Readable by all systems, now and in the future.
- What is well-formedness in syntax?
	- A document is either well-formed or not, a simple boolean.
	- Examples are JSON and XML.
## JSON
- How would you model an empty value in JSON?
	- Not mention it or "null".
- What data-structure does a JSON manifest?
	- A dict, without duplicate keys.
- Can you give a simple JSON document?
	- {"person":null} (**double quotes for both key and value!!!**)
- Is there any syntactic sugar like namespaces, comments, processing instructions in JSON?
	- No.
- Which characters have to be escaped in a JSON string?
	- Special characters in UTF-16, ", \ and white-space characters.
- What are the possible JSON values?
	- Object, array, number, string, boolean, null
## XML
### General
- How would you model an empty element in XML?
	- With <*element* />
- What data-structure does a XML manifest?
	- A tree.
- Can you give a simple XML document?
	- <person/>
- How is the root level different to any other level?
	- There must be exactly one leaf in this level, not more, not less.
- Where are XML attributes defined?
	- They are defined in the start tag e.g. <*element* attribute="true"/>.
- What are the restrictions of attributes?
	- Attribute names must be QNames and unique in the same tag.
- Does order matter?
	- The order of elements matters.
	- The order of attributes does not matter.
### QName, text
- Which characters are legal for XML element names?
	- Alphanumeric, special characters, "-","\_" and "."
- Of those legal characters, which can be at the start?
	- Small and capital letter, "\_".
- Which characters must be escaped in text?
	- <, &
- What is the purpose of "<\!\[CDATA\[\"?
	- You do not have to escape <,&, only the end tag of CDATA. The content in CDATA will be seen as text, no elements in there.
### Comments, Processing Instructions, XML declaration
- What is the "forbidden sequence" of XML-comments?
	- "--" can only be used to close with -->, else you have escape.
- Where can you put comments?
	- Everywhere after the declaration outside the text.
- Who is the receiver of processing instructions?
	- The target defined after <\?, it is also the only one that has to be able to parse the instruction.
- What is an XML declaration there for and how does a sample look like?
	- The XML declaration sets grounds for reading the XML file to follow.
	- <?xml version="1.0" encoding="UTF-8" standalone="no" ?>
- Where can I put a XML declaration?
	- Only at the very beginning of the document, even before comments.
### XML Namespaces
- What is the implicit namespace of the root if nothing has been changed?
	- http://www.w3.org/XML/1998/namespace
- How can the default namespace be changed?
	- To change the namespace in the scope of the tag, you have to change the attribute *xmlns*.
- How are non-default namespaces defined?
	- They are defined like normal attributes registered in the xmlns namespace, they can be used in the same tag as they are created.
- What is the use/effect of namespaces?
	- They are only a shortcut for longer names, **Keep that in mind while having different attributes**.
- Is there a difference between elements and attributes regarding namespaces?
	- Yes, attributes do not have the default namespace function, you can only explicitly define namespaces on them.
- How are namespaces used?
	- <head:person xmlns:head="www.head.com"><head:toe\><\head:person>