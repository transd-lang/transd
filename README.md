# TRANSD
### Welcome to Transd Programming Language

 Transd (short for Transformation and Analysis of Structured Data) is a statically typed, general purpose, multi-paradigm programming language, which:

 * has extended functionality for data processing, such as a built-in data query language for working with tables and collections of records in a database-like fashion;
 * has a very compact implementation (two C++ files with less than 25 KLOCS);
 * uses an innovative execution model - virtual compilation - which doesn't include just-in-time compilation and in the same time offers a good performance.

Currently, Transd is in alpha-stage of development with the planned first 
beta-release in the first quarter of 2022. The syntax of the language is established. The functionality is ready on  85-90%. (There are many examples of [short programs](https://transd.org/doc/rosexamp.html).)

## Code snippets

### List comprehension

```
(with list1 (for i in Range(22) where (not (mod i 3)) project (* i i) )
    (textout list1)
)
// <= [0, 9, 36, 81, 144, 225, 324, 441] 
```

## Documentation (work in progress)

[List of main features of language](https://transd.org/highlights.html)

[Language specification](https://github.com/transd-lang/transd/tree/master/Specification)

[Transd Programming guide](https://transd.org/doc/split/mainguide.html)

[Transd Reference manual](https://transd.org/doc/split/main.html)

## Code samples

[Transd Code Samples](https://transd.org/doc/rosexamp.html)

## REPL command line interpreter

[TREE3](https://github.com/transd-lang/tree3)

## Website

[https://transd.org](https://transd.org)


