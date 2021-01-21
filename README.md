# TransD
### Programming language

Transd is a general purpose programming language with strong emphasis on:

* embeddability
* structured data processing
* compactness of implementation

and as additional design goals:

* speed
* safety
* syntax simplicity and readability

Presently, Transd's most viable sphere of application is C++ programs where it can
be used as an embeddable component for processing structured data of various kinds:
configuration files, log files, tabulated data, records, and so on. All it takes to
embed Transd into a C++ application is to add to sources one "cpp" file and one
"hpp" file. (Yes, the whole Transd implementation is contained in two files.)

Also, the language can be used in the usual way by running programs written on
Transd from the command line with the help of command line launcher.

The language currently is under intensive development, but can already be used in
non-production environments.

A quick example of processing data with Transd can be seen here:

[A quick look into the language for programmers](https://transd-lang.github.io/quick.html)
