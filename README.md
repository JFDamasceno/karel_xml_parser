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

To parse XML:

