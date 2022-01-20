# Quick Introduction to Transd

### "Hello, World!" program

```
#lang transd

MainModule: {

    str1: "Hello",

    _start: (lambda

        (with str2 "World" 

            (textout str1 ", " str2 "!")
    )   ) 
}
// Output: "Hello, World!"
```

This code snippet consists of only few lines, and nonetheless it provides all the
basic information about Transd syntax and program structure. It contains:

1. Language marker;
2. Module definition;
3. Data member variable definition;
4. Scoped variable definition;
5. Member function definition;
6. Function call.

Every file with Transd program code should contain as the first line of code the following directive:

```
#lang transd
```

which marks the start of Transd code.

All the code in a Transd program is contained in _modules_. Every Transd program should
define at least one module:

```
<MODULE_NAME> : {
   <module_definition>
}
```
A module is a basic building block in the Transd program structure. Modules are created at the program's start and exist during the whole running of the program. Modules cannot be deleted or their structure changed.

A module definition consists of data definitions and function definitions.

#### Fields

Data objects in a module definition are termed "data members", or _fields_. Function
objects are termed "member functions", or _methods_.

Fields can be regarded as long-term data storage (variables) in the program: they exist during the whole life-time of the module they contained in. Each field can store only a certain type of data, e.g. strings, or integer numbers. (That is, fields in Transd are typed.) The type is assigned to a field at the spot of its first mentioning in the program (declaration), and this type cannot be changed later. 

A field in module is declared and optionally assigned an initial value as follows:

```
i: Int(), // 'i' is an object of Int() type, but not assigned a value
i1: 5,    // 'i1' is an Int() with the value 5
str1: "Hello"  // 'str1' has the type String() with the value "Hello"
```

#### Methods

The language instructions for manipulating data objects are grouped in blocks called procedures, or functions. Since all code in Transd should be contained within some module, functions just as data objects are defined as members of some module. Module's member functions are termed module's _methods_.


Apart from module's data members Transd has another kind of variables: _local_, or _scoped_ variables.
