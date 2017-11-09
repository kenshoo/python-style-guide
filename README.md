# python-style-guide
This is Kenshoo's Python Style Guide. Using this style guide we hope to acheive the following:

1. Humbly share our knowledge & expeience with the world (and some of our pain too).
1. Assist novice/intermediate developers.
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
     * [Code Layout](#code-layout)
     * [Indentation](#indentation)
     * [New Lines](#new-lines)
  1. [Conditional Expressions](#conditional-expressions)
     * [Truthy vs Falsey](#truthy-vs-falsey)
     * [Single Line](#single-line)
     * [Ternary](#ternary)
     * [Forgiveness vs Permission](#forgiveness-vs-permission)
  1. [Strings](#strings)
     * [Double vs Single Quotes](#double-vs-single-quotes)
     * [Concatenation](#concatenation)
  1. [Collections](#collections)
     * [Slice](#slice)
     * [Comprehensions](#comprehensions)
     * [Builtin Functions](#builtin-functions)
     * [Tuples](#tuples)
     * [Tuple vs List](#tuple-vs-list)
  1. [Imports](#imports)
  1. [Methods/Functions](#methodsfunctions)
     * [Closures](#closures)
     * [Recursion](#recursion)
     * [Arguments/Parameters](#argumentsparameters)
     * [Parameter Passing](#parameter-passing)
     * [Dynamic Parameter Expansion](#dynamic-parameter-expansion)
  1. [Classes](#classes) 
  1. [Exceptions](#exceptions)
  1. [Regular Expressions](#exceptions)

## PEP 20
[The Zen Of Python](https://www.python.org/dev/peps/pep-0020/)


## Naming 

[PEP 8 naming convention](https://www.python.org/dev/peps/pep-0008/#id36)

In general:

 * Use snake_case for modules, methods and variables.
 * Use CamelCase for classes. (Keep acronyms like HTTP, RFC, XML uppercase.)
 * Use SCREAMING_SNAKE_CASE for other constants.
 * The names of predicate methods (methods that return a boolean value) should start with is, does, has, or the likes.
 * The name of predicates should also be positive. (i.e. is_alive, is_empty versus is_not_dead, is_not_empty)
 * Generators (especially comprehension generators), iterators, and other lazy loading objects names should not imply the underlying implementation,
   but rather the result you expect. 

```python

# bad
generate_screaming_list = (word.upper() for word in list_of_words)

for word in generate_screaming_list:
    print(word)
    
# good
screaming_list = (word.upper() for word in list_of_words)

for word in screaming_list:
    print(word)
```


 * Throwaway variables should named with an underscore (`_`).
 * If you feel the need to describe the variable start the name with a variable.

```python
for _ in range(4):
    print 'hi'

for _number in range(4):
    print('hi')
```


## Code Layout

### Indentation

  * Use soft-tabs with a four space-indent
  * Align function parameters either all on the same line or one per line.

```python
# bad
def create_translation(phrase_id, phrase_key, target_locale,
                        value, user_id, do_xss_check, allow_verification)
    ...

# good
def create_translation(phrase_id,
                        phrase_key,
                        target_locale,
                        value,
                        user_id,
                        do_xss_check,
                        allow_verification)
    ...

# good
def create_translation(
    phrase_id,
    phrase_key,
    target_locale,
    value,
    user_id,
    do_xss_check,
    allow_verification
)
    ...

```

### New Lines 

 * There should be two newlines after all your import statements.
 * Don’t include newlines between areas of different indentation (such as around class or method bodies).

```python
# bad
class Foo(object):
    
    def bar(self):
        # body omitted
        
# good
class Foo(object):
    def bar(self):
        # body omitted
```

 * Use a single empty line to break between statements to break up methods into logical paragraphs internally.

```python
def transformorize(self,_car):
  car = manufacture(options)
  t = transformer(robot, disguise)

  car.after_market_mod()
  t.transform(car)
  car.assign_cool_name()

  fleet.add(car)
  return car
end
```

 * End each file with a newline. Don't include multiple newlines at the end of a file.

## Conditional Expressions 

### Truthy vs Falsey 

 * Prefer truthy/falsy checks vs comparing actual values.

"truthy" values are all objects except 

1. `False`
1. `None`
1. `0`
1. `[]` empty lists
1. `()` empty tuples
1. `{}` empty dictionaries
1. an object whose magic method `__bool__` returns falsey(in python 3)
1. an object whose magic method `__nonzero__` returns falsey(in python 2)


```python
# bad
if value > 0:
    ...

# good
if value:
    ...

# bad
if value is 0:
    ...
    
# good
if not value:
    ...
    
# bad   
if len(a_list) > 0:
    ...

# good
if a_list:
    ...
    
# bad sometimes
val = None   
if val is None:
    ...
    
# good
val = None   
if not val:
    ...    
```

 * If you need to check an object if it is `None` and not falsey use the `is` operator

```python
# bad 
if val == None:
    ...
    
# good  
if val is None:
    ...   
```

### Single Line 

 * You can use single line if statements when they are short and simple (like checking truthy statements).
 * Don't use single lines when you using combined conditions.

```python
# bad
if complex_object.with_a_long_name.some_value > complex_object.target_value: return complex_object.with_a_long_name.some_value

# bad
if condition and another_condition: return value

# good
if satisfied: return tip

# Acceptable switch case like conditioning
if very_happy: return tip * 1.5 
if angry: return lip
return tip
```

### Ternary 

 * Avoid the ternary statement except in cases where all expressions are extremely trivial.
 * Avoid multiple conditions in ternaries.
 * Use the ternary operator over if/else/ constructs for single line conditionals.

```python
# bad
return val if condition and other_condition else other_val

# bad
if some_condition:
    return val
else:
    return other_val

# good
return val if some_condition else other_val
```

 * Do not nest ternary expressions.

Prefer if/else constructs in those cases.

```python
# sinful
x = val if some_condition else other_val if nested_condition else something_else

# good
if some_condition:
    x = val
else:
    x = other_val if nested_condition else something_else
```

### Forgiveness vs Permission

It is 'Easier to ask for forgiveness than permission'.

 * When writing code that may throw an exception do not check for that possibility.
 * Assume it will work and catch the exception. 
 * Make sure to catch the expected exception and not all exceptions.

```python
# bad
if some_dict.has_key('a_key'):
    do_something(some_dict['a_key'])
else:
    ...

# good
try:
    do_something(some_dict['a_key'])
exception KeyError:
    ...

# bad
if val != 0:
    10 / val

# good
try:
   10 / val
except ZeroDivisionError:
    ...    
```

## Strings

 * Do not compare strings with `is`.

''is'' has bugs when comparing with strings.

```python
# bad
name is 'bob'

# good
name == 'bob'
```

### Double vs single Quotes

 * Prefer double quotes when your string has multiple words or when formatting (`"hi bob"`).
 * If the string is a single word use single quotes (`'bob'`).

### Concatenation 

 * Prefer string formatting over string concatenation.
 * When formatting prefer mapped formatting ("hi {name}")

Besides for being cleaner this optimizes performance (less work for GC)

```python
# bad
email_with_name = user.name + ' <' + user.email + '>'

# good
email_with_name = "{} <{}>".format(user.name, user.email)

# better
email_with_name = "{name} <{mail}>".format(name=user.name, mail=user.email)

```

 * Avoid appending strings (`+=`) when you need to construct large data chunks. Instead, use `.join`.

```python
# bad
statement = ''
for i in a_list:
    statement += i
    
# good
''.join(i for in a_list)
```

## Collections

There are three major skills to apply to collections.

slicing, comprehensions, and builtin functions.

The purpose of this writing is to automatically associate our collection solutions with
these three tools.

Pushing us away fro using code as such:

```python
    # Bad
    my_new_list = []

    for i in old_list:
        if some_check(i):
            my_new_list.append(i)

```

### Slice

slicing helps a lot, especially with strings.

```python
    # Slice structure
    items[start:end:step]
```

A slice can have up to three attributes.

start - our starting index

end - the index in which to stop (not including)

step - the stepping process

step is interesting because it controls how to jump between items including direction.

i.e.

```python
    items[::-1] # list reversed
    range(10)[2::2] # return all even numbers
    items[4:1:-1] # starting at 5th position go backwards until 2nd position

    def palindrome_check(s):
        midway, is_odd = divmod(len(s), 2)

        return s[midway - 1::-1] == s[midway + is_odd:]

    palindrome_check('hello olleh') # => True
    palindrome_check('noon') # => True
    palindrome_check('joe') # => False    
```


### Comprehensions 

#### list comprehension

syntax is `[ with at least a for statement inside it ]`

```python
    [i for i in range(4)]
```

Mapping

```python
    [mul(2, i) for i in range(4)]
```

You should filter with list comprehension

```python
    [i for i in range(4) if i % 2 == 1]
```

You can chain for loops but make it simple

```python
    [j for i in range(4) for j in range(i)]

    [j for i in range(10) if i % 2 == 0 for j in range(i)]
```

#### generator comprehensions aka lazy iteration 
syntax is ''( with at least a for statement inside it )''

```python
    ( i for i in range(4) )
```

You can do filtering, chaining, all the same things you can do with list comprehension.

However, generators are lazy. Meaning they only evaluate and return a value upon call.

i.e.
```python
    numbers = range(1, 10)

    def check_odd(n):
        print n
        return n % 2 == 0

    non_lazy = [check_odd(i) for i in numbers]
    # print 1
    # print 2
    # print 3
    # print 4
    # print 5
    # print 6
    # print 7
    # print 8
    # print 9
    # executed check_odd nine times

    any(non_lazy) # => True
    
    lazy = (check_odd(i) for i in numbers)
    # didn't execute check_odd yet

    any(lazy) # => True
    # print 1
    # print 2
    # executed check_odd twice

```


### Builtin functions 
There are builtin functions that satisfy most use cases with collections (lists, tuples,
dicts, sets, etc).

Actions on collections like sorting, value checking, and creating new unique lists can be done with builtins.

Refrain from using reduce, map, and filter.

Python's for loops where optimized for high performance (it's comprehension even more so).

Python is not optimized for function calling and recursion.

This means a regular for loop may out perform recursive solutions.


Here is a list of useful builtin functions:

  * all - checks if all values are truthy
  * any - checks if any value is truthy

all and any check if all or any values in list or iterable are truthy

```python
    all([1,2,0]) # => False
    all([1,2,5]) # => True

    any([0,9,0]) # => True
    any([])  # => False

    # Very useful with generator comprehensions
    any((i for i in numbers if i % 2 == 0))

```

  * enumerate - iterate with accompanying climbing value

```python
    for index, name in enumerate(['bob', 'joe', 'sam']):
        print 'index:', index
        print 'name:', name
```

  * filter - filters a list
```python
    filter(lambda i: i % 2, range(10))
```

  * map - creates a list based on values of another list
```python
    map(lambda i: i * 10), range(10))

    # apply two lists to function that takes two parameters 
    map(lambda i,j: i + j, range(1,10), range(100,109))
```

  * max | min - gets max or min value of list
  * sum - adds the values (should be int) of list
  * reduce - with logic creates a single value from list values
  * set - distinct list
  * sorted - sorts values of collection
  * in - finds item in collection

```python
    s = 'bob says hi'

    def check_says_hi(quote):
        return 'says hi' in quote

```
* in is not a builtin function per say rather a very useful operator

Look at the documentation https://docs.python.org/2/library/functions.html

Most of the these builtin functions and others accept a comprehension statement.

Remember comprehension in python is optimized.

Add on that the ability to filter and map.

Therefore, using comprehension syntax is always preferred.

```python
    ' '.join(word for word in speech if allowed(word)) 

    any(word for word in speech if allowed(word))

```


Please Note this doesn't mean never use for loops.
Sometimes a for loop is more appropriate. 
For example when you don't want to return a collection or alter one.

As seen here the for loop benchmarks as more efficient

```{{:devops:pythonportal:test_results.png?200|}}```

```python
    def some(x):
        x + x + x *20

    def comp():
        [some(i) for i in xrange(1000) if i % 2]

    def mapp():
        map(lambda i: some(i) if i % 2 else '', xrange(1000))

    def for_loop():
        for i in xrange(1000):
            if i % 2:
                some(i)

    def test_comp(benchmark):
        # benchmark something
        result = benchmark(comp)

        assert result == 123

    def test_mapp(benchmark):
        # benchmark something
        result = benchmark(mapp)

        assert result == 123

    def test_for_loo(benchmark):
        # benchmark something
        result = benchmark(for_loop)

        assert result == 123

```

### Tuples 

 * Don't use implied tuples

It isn't clear that implied tuples are tuples, so avoid them.
Unless it's part of a DSL or multiple instantiation where our intentions are clear.

```python
# Bad
a_tuple = 9,
a_long_triple = 7, 8, 9

# Good
a, b, c = 7, 8, 9

# Also good
def return_triple():
    return 7, 8, 9

a, b, c = return_triple()

# Good DSL examples
assert False, 'This test was suppose to fail'

print 'Put', 'spaces', 'between', 'these', 'words'
```

### Tuple vs list 
  * Prefer tuples to lists

When deciding which collection type to use try to use a tuple (''(x,)''),
especially if that collection is not going to be changed.

```python
# Bad
def compare_interests(other_persons_interests):
    interests = ['long walks on the beach', 'eating out', 'skateboarding']
    in_common = 0

    for interest in interests:
        if interest in other_persons_interests:
            in_common += 1

    return in_common

# Good
def compare_interests(other_persons_interests):
    interests = ('long walks on the beach', 'eating out', 'skateboarding')
    in_common = 0

    for interest in interests:
        if interest in other_persons_interests:
            in_common += 1

    return in_common

# Even if you are manipulating the contents of a collection still try to use a
# tuple.

# Good
def compare_interests(other_persons_interests):
    interests = ('long walks on the beach', 'eating out', 'skateboarding')
    in_common = ()

    for interest in interests:
        if interest in other_persons_interests:
            in_common += (interest,)

    return in_common
```

## Imports 

There are several ways to import a module in python

```python
1) import package.a           # Absolute import
2) import package.a as a_mod  # Absolute import bound to an alias
3) from package import a      # Alternate absolute import
4) import a                   # Implicit relative import (deprecated, py2 only)
5) from . import a            # Explicit relative import
```

### Do's

 * Always prefer to use 1st syntax - **Absolute Imports**
 * Put all imports in a central module: In __init__.py

```python
from . import a
from . import b
``` 

It has two major flaws: 
it forces all the submodules to be imported, even if you're only using one or two, and you still can't look at any of the submodules and quickly see their dependencies at the top, you have to go sifting through functions.

### Don'ts

 * you shouldn't be using the 4th syntax, since it only works in python2 and runs the risk of clashing with other 3rd party modules.

## Methods/Functions 

### Closures 

 * User-defined functions incur a layer of overhead that hurts performance.
 * Therefore, lambdas and nested functions should be avoided (unless it promotes readability).

### Recursion 

 * Python does not optimize for recursion.

The for loop is, effectively, the same abstraction that recursion provides in functional programming languages.

```python
    # Bad
    arr = map(lambda unit: unit.wanted_field, a_list)
    
    # The best
    arr = [unit.field for unit in a_list]
```

### Arguments/Parameters 

 * Your functions should have up to 3 parameters (not including self and kind).
 * Use splat args (\*args) and double splat keyword args (\*\*kwargs) in method definitions to reduce arguments. 
 * Functions should not receive parameters that they are just suppose to pass to another function. 

If you see a parameter untouched being passed to a few functions, try to put that data into an object or closure who will be passed instead.

```python
    # Bad
    funk1(container, key)
        
    def funk1(container, key, erase=True):
        # Do some other actions
        funk2(container, key, erase)
        
    def funk2(container, key, erase):
        if erase:
            try:
                del container[key]
            except KeyError:
                print "no key:{} to erase".format(key) 
    
    # Good
    funk1(container, key)
        
    def funk1(container, key, erase=True):
        # Do some other actions
        def erase_later():
            if erase: del container[key]
        funk2(erase_later)
        
    def funk2(action):
        try:
            action()
        except KeyError:
            print "no key:{} to erase".format(key) 
```

### Parameter Passing

 * It is best to be explicit when passing arguments.

Especially boolean options.

This adds readability.

```python
def save_file(file_name, log):
    if log:
        print("Saving file {}".format(file_name))
    ...
# Very Bad
save_file("myfile.txt", True)

# Good
save_file("myfile.txt", log=True)

# Better
save_file(file_name="myfile.txt", log=True)  
```

### Dynamic Parameter Expansion

 * You can pass a dynamic list of parameters by using double splat syntax.

## Classes 

 * When defining a class who does not inherit pass `object` as it's inherit class.
 * It is good practice to use private and protected methods. 
 * Try to make classes as thin as possible.

Functions which are not under the responsibilities of a class but their logic is used by the class should not be defined in the class. (RDD Responsibility Driven Development) 

Just put the function in the surrounding scope.

```python
# bad
class Person(object):
    def eat(self, food):
        ...
        self.calories = self.add(self.calories, food.calories)
        ...
    
    def add(self, a,b):
        return a + b
        
# good
class Person(object):
    def eat(self, food):
        ...
        self.calories = add(self.calories, food.calories)
        ...
    
def add(a,b):
    return a + b 
```


## Exceptions 

 * Catch specific exceptions whenever possible
 * Do not use a bare `except:` clause.

```python
# bad
try:
  # an exception occurs here
except:
  # exception handling

# good
try:
  # an exception occurs here
except ValueError:
  # exception handling

# acceptable
try:
  # an exception occurs here
except Exception:
  # exception handling
```

### EAFP 

**EAFP** = Easier to ask forgiveness than permission.

As seen in the conditions section, exception catching is preferred over checking an object before using.

Python's performance does not suffer when exception handling.

Be careful not to abuse this rule in your flow control.

## Regular Expressions 

 * Use single quote raw strings (`r'.+'`) for regex values.

Raw strings preserve escape characters.

This will save you from unexpected behavior.

 * Avoid star wild cards `*`, use plus `+` instead.

Bugs are caused because `*` also allows for zero occurrences.

 * Complex regex should be compiled before using.

The resulting variable's name should clearly define it's purpose.

```python
# Bad
pattern = re.match(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$', '31-11-1985')
pattern.match('31-11-1985')

# Good
valid_date_pattern = re.compile(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$')
valid_date_pattern.match('31-11-1985')
```

### Usage

 * Regex is not preferable in python.

There are plenty of tools which are preferred (i.e. `in`, `startswith`, `endswith`, `isdigit`, `istitle`, and more).

```python
# Bad
is_bob_pattern = re.compile(r'^Bob')
is_bob_pattern.match(name)

has_a_qu_pattern = re.compile(r'qu', re.I)
has_a_qu_pattern.match(word)

# Good
name.startswith('Bob')

'qu' in word.lowercase()
```



## Main Contributers:
* [Avraf](https://github.com/avraf)
* [Eyalstoler](https://github.com/eyalstoler)
