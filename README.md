# python-style-guide
This is Kenshoo's Python Style Guide. Using this style guide we hope to acheive the following:

1. Humbly share our knowledge & expeience with the world (and some of our pain too).
1. Assist novice/intermeddiate developers.
1. Generate healthy discussion which will benefit all.
1. Admit and accept we may be wrong and change per the community's comments.

We **DO NOT** wish the following:
1. Say/claim/Imply there is a "right" or "wrong" way to do it.
1. Claim this style guide is inclusive.
1. Introduce ourselves as some kind of language "experts". 

Inspired by many style guides but mainly from [Airbnb](https://github.com/airbnb)'s Style-Guide "style" :-).

**WORK IN PROGRESS. Please be patient.**

## Table of Contents

  1. [PEP 20](#pep-20)
  1. [Naming](#naming)
    1. [Whitespace](#whitespace)
    1. [Indentation](#indentation)
    1. [Newlines](#newlines)
  1. [Line Length](#line-length)
  1. [Conditional Expressions](#conditional-expressions)
    1. [Truthy vs.Falsy](#truthy-vs-falsy)
    1. [Single Line](#single-line)
    1. [Ternary](#ternary)
    1. [Forgiveness vs Permission](#forgiveness-vs-permission)
  1. [Strings](#strings)
    1. [Double vs Single Quotes](#double-vs-single-quotes)
    1. [Concatenation](#concatenation)
  1. [Collections](#collections)
    1. [Slice](#slice)
    1. [Comprehensions](#comprehensions)
    1. [Comprehensions](#comprehensions)
    1. [Builtin Functions](#builtin-functions)
    1. [Tuples](#Tuples)
    1. [Tuple vs List](#tuple-vs-list)
  1. [Imports](#imports)
  1. [Methods/Functions](#methods-functions)
    1. [Closures](#closures)
    1. [Recursion](#recursion)
    1. [Arguments/Parameters](#arguments-parameters)
  1. [Classes](#classes) 
  1. [Exceptions](#exceptions)
  1. [Regular Expressions](#exceptions)

## PEP 20
[The Zen Of Python](https://www.python.org/dev/peps/pep-0020/)

## Naming
If you really want to be Pythonic, we recommend to first go over [PEP](https://www.python.org/dev/peps/pep-0008/) 8 to start with. We definately have some more to add.

* Use snake_case for modules, methods and variables.
* Use CamelCase for classes. (Keep acronyms like HTTP, RFC, XML uppercase.)
* Use SCREAMING_SNAKE_CASE for other constants. Keep these always at top of the file right after imports
* Names of predicate methods (methods that return a boolean value) should always start with is, does, has, or the likes.
* Don't use name abbreviations (interpeter doesn't mind and it won't cost you with perfoemance):
```python
# bad
params
#good
parameters

# bad
inst
#good
instance
```


* In general, Method & Generator names should not imply the underlying implementation

```python
def get_template_parameters
```






## Main Contributers:
* [Avraf](https://github.com/avraf)
* [Eyalstoler](https://github.com/eyalstoler)
