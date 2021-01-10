# Big Data Week 04 questions

## Usage

<details><summary>Why should we have a unified syntax? </summary>

- Readable by all systems, now and in the future.

</details>
<details><summary>What is well-formedness in syntax? </summary>

- A document is either well-formed or not, a simple boolean.
- Examples are JSON and XML. 

</details>
	
## JSON
<details><summary>How would you model an empty value in JSON? </summary>

- Not mention it or "null". 

</details>
<details><summary>What data-structure does a JSON manifest? </summary>

- A dict, without duplicate keys. 

</details>
<details><summary>Can you give a simple JSON document? </summary>

- {"person":null} (**double quotes for both key and value!!!**) 

</details>
	
<details><summary>Is there any syntactic sugar like namespaces, comments, processing instructions in JSON? </summary>

- No. 

</details>
<details><summary>Which characters have to be escaped in a JSON string? </summary>

- Special characters in UTF-16, ", \ and white-space characters.

</details>	
<details><summary>What does a number in JSON correspond to in Python? </summary>

- JSON is programming language independent and the standard only defines conformance, not how to interpret the text e.g. if an object is a list or an array.

</details>
<details><summary>What are possible JSON value types? </summary>

- A JSON value can be one of:
	- object
	- array 
	- number 
	- string
	- literal name token

</details>
<details><summary>What are the tokens in JSON?</summary>

- JSON text is formed out of strings, numbers and 9 tokens.

- 6 Structural tokens:
	- [
	- {
	- ]
	- }
	- :
	- ,

- 3 literal name tokens:
	- true
	- false
	- null

</details>
<details><summary>How are numbers represented in JSON? </summary>

![JSON numbers](../images/04_JSON_number.PNG)

</details>

## XML
### General
<details><summary>How would you model an empty element in XML? </summary>

- With <*element* />

</details>
<details><summary>What data-structure does a XML manifest? </summary>

- A tree.

</details>
<details><summary>Can you give a simple XML document? </summary>

- \<person\/\>

</details>
<details><summary>How is the root level different to any other level in XML? </summary>

- There must be exactly one leaf in this level, not more, not less.

</details>
<details><summary>Where are XML attributes defined? </summary>

- They are defined in the start tag e.g. <*element* attribute="true"/>. 

</details>	
<details><summary>What are the restrictions of attributes? </summary>

- Attribute names must be QNames and unique in the same tag.

</details>	
<details><summary>Does order matter in XML? </summary>

- The order of elements matters.
- The order of attributes does not matter. 

</details>

### QName, text
<details><summary>Which characters are legal for XML element names? </summary>

- Alphanumeric, special characters, "-","\_" and "." 

</details>
<details><summary>Of those legal characters, which can be at the start? </summary>

- Small and capital letter, "\_".

</details>
<details><summary>Which characters must be escaped in text? </summary>

- <, &

</details>	
<details><summary>What is the purpose of "<\!\[CDATA\[\"? </summary>

- You do not have to escape <,&, only the end tag of CDATA. The content in CDATA will be seen as text, no elements in there.

</details>
<details><summary>Is the tag "aTag" the same as "atag" in XML? </summary>

- No, Tags are case-sensitive.

</details>

### Comments, Processing Instructions, XML declaration

<details><summary>What is the "forbidden sequence" of XML-comments? </summary>

- "--" can only be used to close with -->, else you have escape. 

</details>
<details><summary>Where can you put comments? </summary>

- Everywhere after the declaration outside the text.

</details>

<details><summary>Who is the receiver of processing instructions? </summary>

- The target defined after <\?, it is also the only one that has to be able to parse the instruction. 

</details>
<details><summary>What is an XML declaration there for and how does a sample look like? </summary>

- The XML declaration sets grounds for reading the XML file to follow.
- \<\?xml version="1.0" encoding="UTF-8" standalone="no" \?\>

</details>
<details><summary>Where can I put a XML declaration? </summary>

- Only at the very beginning of the document, even before comments.

</details>

### XML Namespaces
<details><summary>What is the implicit namespace of the root if nothing has been changed? </summary>

- http://www.w3.org/XML/1998/namespace

</details>
<details><summary>How can the default namespace be changed? </summary>

- To change the namespace in the scope of the tag, you have to change the attribute *xmlns*.

</details>	
<details><summary>How are non-default namespaces defined? </summary>

- They are defined like normal attributes registered in the xmlns namespace, they can be used in the same tag as they are created.

</details>
<details><summary>What is the use/effect of namespaces? </summary>

- They are only a shortcut for longer names, **Keep that in mind while having different attributes**.

</details>	
<details><summary>Is there a difference between elements and attributes regarding namespaces? </summary>

- Yes, attributes do not have the default namespace function, you can only explicitly define namespaces on them. 

</details>
<details><summary>How are namespaces used? </summary>

- <head:person xmlns:head="www.head.com"><head:toe\><\head:person>

</details>