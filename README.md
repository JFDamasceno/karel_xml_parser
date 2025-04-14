# FANUC Karel XML Parser Library
XML parser library for FANUC Karel.

This repository contains a custom XML parsing library written in **Karel**, the programming language used for FANUC robot controllers. The library allows for parsing and extracting data from XML-formatted strings directly within a Karel environment.

## Features

- Parses well-formed XML strings into structured data.
- Handles nested XML tags and attributes.
- Supports inner text extraction.
- Provides utility routines to retrieve string, integer, real, and frame data.
- Lightweight and designed to run on FANUC robot controllers with limited memory.

## Contents

This library consists of:
- `XML_PARSER.kl`   – The main KAREL source file.
- `XML_PARSER.h.kl` – The header file with routine declarations.

## Data Structures

The library uses custom Karel `STRUCTURE` types to store parsed XML:

- `XmlStruct`: Core structure holding tag, inner text, parent/root index.
- `XmlElement`: Flattened element list with tag and inner text.
- `xmlAttrStrct`: Key-value pair representation for attributes.
- `EulerAngles`: Stores custom rotation component tag mappings (`W`, `P`, `R`) for xmlGetFrame.

## Public Routines

| Routine         | Description                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| `xmlParse`      | Main parsing function. Converts XML array of strings to structured data.          |
| `xmlClear`      | Resets parsed element storage.                                                    |
| `xmlGetStr`     | Retrieves a string value by tag.                                                  |
| `xmlGetInt`     | Retrieves an integer value by tag.                                                |
| `xmlGetReal`    | Retrieves a real number value by tag.                                             |
| `xmlGetFrame`   | Retrieves a complete robot frame (X, Y, Z, W, P, R) using configurable tag names. |

## Private Routines
| Routine         | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| `parser_init`   | Initializes and clears parsing buffers and structures.                      |
| `SaveData`      | Transfers valid parsed data into `xmldata` for retrieval.                   |
| `xmlpop`        | Removes an element from `xmldata` by index.                                 |

## Usage

### 1. Include the Header
In your KAREL source file, include the XML_PARSER header:

```karel
%INCLUDE XML_PARSER.h
```

This allows you to call the parser routines.

---

### 2. Prepare XML Input

Pass the XML content as an array of strings. 

The strings in this array must be declared with size 254.

Example of a single element :

```karel
xml_data[1] = '<robot>'
xml_data[2] = '<id>12345</id>'
xml_data[3] = '<name>Robot1</name>'
xml_data[4] = '<frame><X>100.0</X><Y>200.0</Y><Z>300.0</Z>'
xml_data[5] = '<A>180.0</A><B>0.0</B><C>90.0</C></frame>'
xml_data[6] = '<part size="31.01" color="blue" />'
xml_data[7] = '</robot>'
```

It is also possible to read multiple XMLs in the same array.

```karel
xml_data[1] = '<robot><id>12345</id><name>Robot1</name><frame><X>100.00</X><Y>200.00</Y><Z>300.00</Z><A>180.00</A><B>0.00</B><C>90.00</C></frame><part size="31.01" color="blue" /></robot><robot><id>12345</id><name>Robot1</name><frame><X>100.00</X><Y>200.00</Y><Z>300.00</Z'
xml_data[2] = '><A>180.00</A><B>0.00</B><C>90.00</C></frame><part size="31.01" color="blue" /></robot>'
```
---

### 3. Parse the XML

Call `xmlParse` with the array:

```karel
success = xmlParse(xml_data)
```

Returns `TRUE` if parsing succeeds.

---

### 4. Access Parsed Values


To access parsed XML data, use one of the accessor functions:

- `xmlGetInt(tag, intVar)`
- `xmlGetStr(tag, strVar)`
- `xmlGetReal(tag, realVar)`
- `xmlGetFrame(tag, frameVar)`

These functions will search the parsed data for the tag provided and return its value.

Provide the full tag path to the desired value as a string.

Use the element names as they appear in the XML, separated by `/` for nested tags.

Attributes are treated as nested tags.


Example:

```karel
success = xmlGetInt('robot/id', id)
success = xmlGetStr('robot/name', name)
success = xmlGetFrame('robot/frame', frame)
success = xmlGetReal('robot/part/size', size)
```

Returns `TRUE` if it succeeds.

Each time you use a xmlGet function, the corresponding tag and its value are automatically removed from the internal parsed XML data. This is similar to a stack pop operation and helps ensure that:

- Each tag is only accessed once
- You avoid accidentally reading the same data multiple times
- The parsed data array stays clean as you process it

Example for a single xml string: 

```karel
-- Get and remove the 'robot/name' tag from the parsed XML
success = xmlGetStr('robot/name', robotName) --success returns TRUE

-- Trying to access 'robot/name' again will now fail, since it was removed
success = xmlGetStr('robot/name', anotherName)  -- success returns FALSE
```
---

### 5. Clean Up

To clear all previously parsed data, call:

```karel
xmlClear
```

---

### Notes

- Tag paths use forward slash notation (e.g., `robot/id`).
- XML must be well-formed. Nested tags must be closed properly.
- Special namespaces or xml comments are not supported.
- Max number of elements supported is 100 by default (can be changed in source).

## License
 This library is released under the MIT License. Feel free to use and adapt it for your projects.
 
## Contributions
 1. Fork
 2. Create feature branch
 3. Commit changes
 4. Push to the branch
 5. Create new Pull Request

