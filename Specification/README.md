# Transd Language Specification

(Working draft)

## 1. Source code

Source code of Transd programs is stored in text files in UTF-8 encoding without Byte Order Mark (BOM).

Each file with Transd source code should contain a line with the Transd source code marker before any program text:

```
#lang transd
```

The standard file name extension for Transd source files is `.td`.

## 2. Structure of Transd program

A Transd program on the top level consists of a set (a list, where the order of elements is insignificant) of compilation units. A Transd _compilation unit_ is a set of _unit members_: a comma separated list of unit _member declarations_. Each declaration is a colon separated name/value pair. The list is enclosed in curly braces and prepended with the unit header.

```
program := compilation_unit ["," compilation_unit]+
compilation_unit := unit_header ": {" unit_member_list "}"
unit_member_list := unit_member_declaration ["," unit_member_declaration]+
unit_member_declaration := MEMBER_NAME ":" member_definition
member_name := IDENTIFIER
member_definition := field_definition | method_definition
```

Compilation units are defined in source files in arbitrary order. A source file can contain any
number of compilation units. A program can be defined in any number of source files.

Compilation units are of two kinds: modules and classes. Syntactically they differ in the unit header.

```
unit_header := class_header | module_header
class_header := [access_mode_specifier] "class " UNIT_NAME
module_header := [access_mode_specifier] ["module "] UNIT_NAME
access_mode_specifier := "public" | "secure" | "private"
```

### Main module and start method

Each Transd program should contain a module having a _start method_ assigned the role of program's "entry point", that is, the function from which the program execution begins (an analog of "main" function in C). The standard name for such method is `_start`.

The module containing start method is called _main module_. A program can contain exactly one main module. The standard name for main module is `MainModule`.


### Examples
An example of a program having one class and one module.

```
#lang transd

class Hello: {
    s: "Hello, World!",
    greet: (lambda (textout s))
}

module MainModule: {
    hello: Hello(),
    _start: (lambda (greet hello)) // <= "Hello, World!"
}
```


## Modules
Modules in Transd are containers for all program code, apart from class definitions. A program can consists of any number (one and greater) of modules. 

Modules bear properies that make them similar both to namespaces and to objects.

Modules resemble objects because they have members, which are of two kinds: data members and function members. (In Transd documentation, data members are termed _fields_, function members are termed _methods_.) Modules have access modes and can restrict access to their members from other modules. Also, modules can have initialization functions, that run at loading modules.

Modules resemble namespaces because their structure cannot be changed at run-time: their members cannot be removed or added. Also modules cannot be referenced as objects: they cannot be copied, assigned, passed as arguments to or returned from functions. Although, modules can be dynamically loaded and unloaded at runtime. If anything, modules can be compared to strengthened singleton objects.

Each module has a name, which should be unique in the whole program.

Modules are only top-level and cannot be nested.

### Visibility and access mode

Within one module all module members are visible and accessible to each other by their unqualified names.

__Example__

```
MainModule: {
  a: 0, b: 0,
  sum: (lambda (ret (+ a b))) 
  _start: (lambda (= a 1) (= b 2) (textout (sum))) // <= 3
}
```


* By default, module members are not visible and not accesible to other modules.
* By definig access control rules for a module, the programmer can allow access to some or to all module members for some or for all other modules.
* In order to access members in other modules, the "import" directive needs to be placed within the accessing module definition.
* There are three standard access modes for modules, which can be used for granting or restricting access to module members without defining access rules:
  1. _public_ - allow access to all module members (fields and methods).
  2. _private_ - deny access to module's fields, allow access to module's methods.
  3. _secure_ - deny access to all module's members.


## Variables
Variable is a name, associated with some data object in the program. Another term for such association is _binding_: it is said, that a variable is _bound_ to an object.

It's called a variable because the object that a variable is bound to can have different values (in case if the object is not constant), and because a variable during its existence can be bound to different objects (of the same type).

There are two sorts of variables in Transd: data members and scoped variables.

### Data member variables

Data members are part of some object and exist during the whole lifetime of that object. Module data members exist for the whole running of the program just as modules themselves.

Data members are introduced as fields in the definitions of objects.

Example:

```
MainModule: {
    s: String(),

    _start: (λ 
        (= s "Hello!")
	(textout s) // <= "Hello!"
    )
}
```
A module field `s` in this example is a data member variable, which originally is bound to an empty string, and then this string is assigned a new value "Hello!".

### Scoped variables

Scoped variables are introduced with the scope operator `(with)`. Their life time is limited to the execution time of the body of the scope operator.

Example:

```
MainModule: {
    s: "World!",

    _start: (λ 
        (with s "Hello,"
            (textout s) // <= "Hello,"
        )
        (textout s) // <= "World!"
    )
}
```

## Functions
Function is a block of code containing a sequence of commands and returning some value.
There are two kinds of syntax rules related to functions: how functions are defined and how they are called.

### Function definition
All functions in Transd are defined as methods (or member functions) of some object (just as all non-local variables are defined as a data member of some object).

The grammar of method definition is as follows:

```
method_definition := METHOD_NAME ": (lambda" [method_signature] method_body ")"
method_signature := parameters ["->" RETURN_TYPE]
parameters := [PAR_NAME PAR_TYPE]+
method_body := [method_call]*
```

### Function call



## Classes and objects
 Object in Transd is a set of data variables along with set of functions that operate on those variables. From this description one can conclude, that objects and modules in Transd are somewhat similar. And they indeed are similar, the modules being, actually, special objects. The differences of modules from plain objects make them a separate topic of documentation, and what further is said about objects and classes is not neccesseraly relates to modules.

Classes in Transd, like in many other languages, are templates for creating objects. An object is, basically, a collection of fields (data members) and methods (function members). A class can be regarded as an example object, whose copies are called _instances_ of that class.

Classes use the same mechanism as modules for restricting access to their members, namely, an access mode for the whole class, and/or an access control list when a more fine grained member access control is needed.

The grammar for defining a class is as follows:

```
class_definition := class_definition_header ": {" class_definition_body  "}"
class_definition_header := [access_mode_specifier] "class" CLASS_NAME
access_mode_specifier := "public" | "secure" | "private"
class_definition_body := [member_list] [access_control_list]
member_list := member_declaration ["," member_declaration]*
member_declaration := field_declaration | method_declaration
field_declaration := FIELD_NAME ":" field_definition
method_declaration := METHOD_NAME ":" method_definition
field_definition := 
access_control_list := acl_header ": {" acl_rules_list "}"
acl_header := "access" ["allow" | "deny"]
acl_rules_list := acl_rule ["," acl_rule]*
acl_rule :=  MEMBER_NAME ":" modules_list
```




