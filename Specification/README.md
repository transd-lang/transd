# Transd Language Specification

## 1. Source code

Source code of Transd programs is stored in text files in UTF-8 encoding without Byte Order Mark (BOM).

Each file with Transd source code should contain a line with the Transd source code marker before any program text:

```
#lang transd
```

The standard file name extension for Transd source files is `.td`.

## 2. Structure of Transd program

A Transd program on the top level consists of a set (a list, where the order of elements is insignificant) of compilation units. A Transd compilation unit is a set of unit members. This set is a comma separated list of unit member declarations. Each declaration is a colon separated name/value pair. The list is enclosed in curly braces and prepended with the unit header.

```
program := compilation_unit ["," compilation_unit]+
compilation_unit := unit_header ": {" unit_member_list "}"
unit_member_list := unit_member_declaration ["," unit_member_declaration]+
unit_member_declaration := member_name ":" member_definition
member_name := identifier
member_definition := field_definition | method_definition
```

Compilation units are defined in source files in arbitrary order. A source file can contain any
number of compilation units. A program can be defined in any number of source files.

Compilation units are of two kinds: modules and classes. Syntactically they differ in the unit header.

```
unit_header := class_header | module_header
class_header := [access_mode] "class " unit_name
module_header := [access_mode] ["module "] unit_name
access_mode := "public " | "secure " | "private "
```



