# TRANSD
### Welcome to Transd Programming Language

 Transd (short for Transformation and Analysis of Structured Data) is a statically typed, general purpose, multi-paradigm programming language, which:

 * has extended functionality for data processing, such as a built-in data query language for working with tables and collections of records in a database-like fashion;
 * has a very compact implementation (two C++ files with less than 25 KLOCS);
 * uses an innovative execution model - virtual compilation - which doesn't include just-in-time compilation and in the same time offers a good performance.

The current stage of Transd development is **beta** of the first production version, which is planned for release in the first quarter of 2023.

## Code snippets

### Iterating a vector

```
(with v [1,2,3,4,5,6] 

    (for i in v do (textout i " ")

    (textout "\n")

    (for i in v where (not (mod i 2)) do (textout i " ")
)
// <= 1 2 3 4 5 6
// <= 2 4 6
```

### List comprehension
The 'for' statement is used not only for iterations, but for list comprehensions as well.

```
(with list1 (for i in Range(22) where (not (mod i 3)) project (* i i) )

    (textout list1)
)
// <= [0, 9, 36, 81, 144, 225, 324, 441] 
```

### Classes and lambdas

```
class Speaker : {

    say: Lambda<String String>(),

    @init: (λ f Lambda<String String>() (rebind say f )),

    call: (λ s String() (ret (exec say s)))
}

MainModule: {

     greeter: Lambda<String String>(λ s String() (ret (+ "Hello, " s "!"))),

    _start: (λ 

        (with sp Speaker( greeter )

            (textout (call sp "World"))
    )   )
}
// Output: Hello, World!
```

### Built-in data query language

```
#lang transd
MainModule : {
  tabdata:
    "Name,Salary:Int,Department
     Alice,10000,IT
     Susan,25000,HR
     Bob,30000,Marketing",
  
  table: Table(),

   _start: (λ
    (load-table table tabdata)
    (build-index table "Salary")

    (with rows (tsd tabl 
                  select: ["Name", "Department"]
                  as: [[String(),String()]]
                  where: "Salary > 20000"
                  sortby: "Name")

      (for row in rows do (lout row)))
  	  // OUTPUT:
          // <= ["Bob", "Marketing"]
          // <= ["Susan", "HR"]
   )
}
```

## Documentation (work in progress)
[Transd in 5 minutes](https://github.com/transd-lang/transd/tree/master/TransdInFiveMin)

[List of main features of language](https://transd.org/highlights.html)

[Quick introduction to Transd](https://github.com/transd-lang/transd/tree/master/QuickIntro)

[Language specification](https://github.com/transd-lang/transd/tree/master/Specification)

[Transd Programming guide](https://transd.org/doc/split/mainguide.html)

[Transd Reference manual](https://transd.org/doc/split/main.html)

## Code samples

[Transd Code Samples](https://transd.org/doc/rosexamp.html)

[Chess Engine in Transd](https://github.com/al-berger/SFTD)

## REPL command line interpreter

[TREE3](https://github.com/transd-lang/tree3)

## Website

[https://transd.org](https://transd.org)


