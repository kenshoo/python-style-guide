====== python Style Guide ======

python Style Guide.

This style guide should force discussion.

Anyone can add or change it's content.

When adding or changing a style please add why it's a better style or why 
alternatives are not.


===== The Zen of Python =====
  * Beautiful is better than ugly.
  * Explicit is better than implicit.
  * Simple is better than complex.
  * Complex is better than complicated.
  * Flat is better than nested.
  * Sparse is better than dense.
  * Readability counts.
  * Special cases aren't special enough to break the rules.
  * Although practicality beats purity.
  * Errors should never pass silently.
  * Unless explicitly silenced.
  * In the face of ambiguity, refuse the temptation to guess.
  * There should be one-- and preferably only one --obvious way to do it.
  * Although that way may not be obvious at first unless you're Dutch.
  * Now is better than never.
  * Although never is often better than *right* now.
  * If the implementation is hard to explain, it's a bad idea.
  * If the implementation is easy to explain, it may be a good idea.
  * Namespaces are one honking great idea -- let's do more of those!

===== Naming =====
use [[https://www.python.org/dev/peps/pep-0008/#id36|this documentation]] as describe in PEP 8.

In general:

Use snake_case for modules, methods and variables.

Use CamelCase for classes. (Keep acronyms like HTTP, RFC, XML uppercase.)

Use SCREAMING_SNAKE_CASE for other constants.

The names of predicate methods (methods that return a boolean value) should start with is, does, has, or the likes.

The name of predicates should also be positive. (i.e. is_alive, is_empty versus is_not_dead, is_not_empty)

Generators (especially comprehension generators), iterators, and other lazy loading objects names should not imply the underlying implementation,
but rather the result you expect. 

<code python>

# bad
generate_screaming_list = (word.upper() for word in ['a', 'list', 'of', 'words'])

for word in generate_screaming_list:
    print(word)
    
# good
screaming_list = (word.upper() for word in ['a', 'list', 'of', 'words'])

for word in screaming_list:
    print(word)
    
# bad
def build_stack_template_parameters(stack_files):
    for file in stack_files:
        if file.has_no_default:
            yield file['parameters']
            
for parameters in build_stack_template_parameters(stack_files):
    ...
    

# good
def stack_template_parameters(stack_files):
    for file in stack_files:
        if file.has_no_default:
            yield file['parameters']
            
for parameters in stack_template_parameters(stack_files):

</code>


Name throwaway variables `_`(variables you don't need).

<code python>
for _ in range(4):
    print 'hi'
</code>




===== Whitespace =====

==== Indentation ====

  * Use soft-tabs with a four space-indent

  * Align function parameters either all on the same line or one per line.

<code python>
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

</code>

==== New Lines ====

There should be two newlines after all your import statements.

Don’t include newlines between areas of different indentation (such as around class or method bodies).

<code python>
# bad
class Foo(object):
    
    def bar(self):
        # body omitted
        
# good
class Foo(object):
    def bar(self):
        # body omitted
</code>

Use a single empty line to break between statements to break up methods into logical paragraphs internally.
<code python>
def transformorize(self,_car):
  car = manufacture(options)
  t = transformer(robot, disguise)

  car.after_market_mod()
  t.transform(car)
  car.assign_cool_name()

  fleet.add(car)
  return car
end
</code>

End each file with a newline. Don't include multiple newlines at the end of a file.

===== Conditional Expressions =====

==== Checking Truthy/Falsy ====

Prefer truthy/falsy checks vs comparing actual values.

<code python>
# bad
if value > 0:
    ...

# bad
if value is 0:
    ...
    
# bad   
if len(a_list) > 0:
    ...
    
# bad
val = None   
if val is None:
    ...

# good
if value:
    ...
    
# good
if not value:
    ...
    
# good
if a_list:
    ...
    
# good
val = None   
if not val:
    ...
    
</code>

==== Single Line Conditionals ====


You can use single line if statements when they are short and simple (like checking truthy statements).

Don't use single lines when you using combined conditions.

<code python>
# bad
if complex_object.with_a_long_name.some_value > complex_object.target_value: return complex_object.with_a_long_name.some_value

# bad
if condition and another_condition: return value

# good
if satisfied: return tip

# Even switch case like conditioning
if very_happy: return tip * 1.5 
if angry: return lip
return tip

</code>

==== Ternary statements ====


Avoid the ternary statement except in cases where all expressions are extremely trivial.

Avoid multiple conditions in ternaries.

Ternaries are best used with single conditions.

However, do use the ternary operator over if/else/ constructs for single line conditionals.

<code python>
# bad
return val if condition and other_condition else other_val

# bad
if some_condition:
    return val
else:
    return other_val

# good
return val if some_condition else other_val

</code>

This also means that ternary operators must not be nested.

Prefer if/else constructs in those cases.

<code python>
# bad
x = val if some_condition else other_val if nested_condition  else something_else

# good
if some_condition:
    x = val
else:
    x = other_val if nested_condition else something_else

</code>

==== Forgiveness vs Permission ====

It is 'Easier to ask for forgiveness than permission'.

When writing code that may fail because an attribute/key doesn't exist or another reason, don't check it first and then handle.

Just assume it will work and catch the exception if not. 

But make sure to catch the expected exception and not all exceptions.

<code python>
# bad
if some_dict.has_key('a_key'):
    do_something(some_dict['a_key'])
else:
    ...

if val != 0:
    10 / val

# good
try:
    do_something(some_dict['a_key'])
exception KeyError:
    ...

try:
   10 / val
except ZeroDivisionError:
    ...

    
</code>

===== Strings =====

Do not compare strings with ''is''.

''is'' has bugs when comparing with strings.

<code python>
# bad
name is 'bob'

# good
name == 'bob'
</code>

==== Double vs single Quotes ====

Prefer double quotes (''"b"'') when your string is multiple words or formatting.

If the string is a single word use single quotes ('''b''').

==== Concatenation ====

Prefer string formatting instead of string concatenation:
<code python>
# bad
email_with_name = user.name + ' <' + user.email + '>'

# good
email_with_name = "{} <{}>".format(user.name, user.email)

# better
email_with_name = "{name} <{mail}>".format(name=user.name, mail=user.email)

</code>

Avoid using ''+'' when you need to construct large data chunks. Instead, use ''.join''.
<code python>
# bad
statement = ''
for i in a_list:
    statement += i
    
# good
''.join(i for in a_list)
 
</code>

===== Collections =====

There are three major skills to apply to collections.

slicing, comprehensions, and builtin functions.

The purpose of this writing is to automatically associate our collection solutions with
these three tools.

Pushing us away fro using code like so:

<code python>
    # Bad
    my_new_list = []

    for i in old_list:
        if some_check(i):
            my_new_list.append(i)

</code>

==== Slice ====

slicing helps a lot, especially with strings.

<code python>
    # Slice structure
    items[start:end:step]
</code>

A slice can have up to three attributes.

start - our starting index

end - the index in which to stop (not including)

step - the stepping process

step is interesting because it controls how to jump between items including direction.

i.e.

<code python>

    items[::-1] # list reversed
    range(10)[2::2] # return all even numbers
    items[4:1:-1] # starting at 5th position go backwards until 2nd position

    def palindrome_check(s):
        midway, is_odd = divmod(len(s), 2)

        return s[midway - 1::-1] == s[midway + is_odd:]

    palindrome_check('hello olleh') # => True
    palindrome_check('noon') # => True
    palindrome_check('joe') # => False
    
</code>


==== Comprehensions ====


=== list comprehension ===

syntax is ''[ with at least a for statement inside it ]''

<code python>
    [i for i in range(4)]
</code>

being creative

<code python>
    [mul(2, i) for i in range(4)]
</code>

you can filter with list comprehensions
<code python>
    [i for i in range(4) if i % 2 == 1]
</code>

you can chain for loops
<code python>
    [j for i in range(4) for j in range(i)]

    [j for i in range(10) if i % 2 == 0 for j in range(i)]
</code>

=== generator comprehensions aka lazy iteration ===

syntax is ''( with at least a for statement inside it )''

<code python>
    ( i for i in range(4) )
</code>

You can do filtering, chaining, all the same things you can do with list comprehension.

However, generators are lazy. Meaning they only evaluate and return a value upon call.

i.e.
<code python>
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

</code>


==== Builtin functions ====

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

<code python>
    all([1,2,0]) # => False
    all([1,2,5]) # => True

    any([0,9,0]) # => True
    any([])  # => False

    # Very useful with generator comprehensions
    any((i for i in numbers if i % 2 == 0))

</code>

  * enumerate - iterate with accompanying climbing value

<code python>
    for index, name in enumerate(['bob', 'joe', 'sam']):
        print 'index:', index
        print 'name:', name
</code>

  * filter - filters a list
<code python>
    filter(lambda i: i % 2, range(10))
</code>

  * map - creates a list based on values of another list
<code python>
    map(lambda i: i * 10), range(10))

    # apply two lists to function that takes two parameters 
    map(lambda i,j: i + j, range(1,10), range(100,109))
</code>

  * max | min - gets max or min value of list
  * sum - adds the values (should be int) of list
  * reduce - with logic creates a single value from list values
  * set - distinct list
  * sorted - sorts values of collection
  * *in - finds item in collection

<code python>
    s = 'bob says hi'

    def check_says_hi(quote):
        return 'says hi' in quote

</code>
* in is not a builtin function per say rather a very useful operator

Look at the documentation https://docs.python.org/2/library/functions.html

Most of the these builtin functions and others accept a comprehension statement.

Remember comprehension in python is optimized.

Add on that the ability to filter and map.

Therefore, using comprehension syntax is always preferred.

<code python>
    ' '.join(word for word in speech if allowed(word)) 

    any(word for word in speech if allowed(word))

</code>


Please Note this doesn't mean never use for loops.
Sometimes a for loop is more appropriate. 
For example when you don't want to return a collection or alter one.

As seen here the for loop benchmarks as more efficient

{{:devops:pythonportal:test_results.png?200|}}

<code python>
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

</code>
==== Tuples vs list ====

  * Prefer tuples to lists

When deciding which collection type to use try to use a tuple (''(x,)''),
especially if that collection is not going to be changed.

<code python>
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
</code>

==== Tuples ====

Single element tuples should have a comma after first item

This rule avoids unexpected behavior
<code python>
# Unexpected behavior
print '{}'.format((9)) # => 9
print '{}'.format((9,)) # => (9,)

# Even worse
x = (9)
y = (9,)

print '{}'.format(x) # => 9
print '{}'.format(y) # => (9,)

def action():
    # Assumed to return a tuple with one string item
    return ('Some cool string')

print action().__class__ #=> <type 'str'>

</code>

Don't use implied tuples

It isn't clear that implied tuples are tuples, so avoid them.
Unless it's part of a DSL or multiple instantiation where our intentions are clear.

<code python>
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
</code>

====== Imports ======
There are several ways to import a module in python
<code>
1) import package.a           # Absolute import
2) import package.a as a_mod  # Absolute import bound to different name
3) from package import a      # Alternate absolute import
4) import a                   # Implicit relative import (deprecated, py2 only)
5) from . import a            # Explicit relative import
</code>

==== Do's ====
  * Always prefer to use 1st syntax - **Absolute Imports**
  * Put all imports in a central module: In __init__.py<code>
from . import a
from . import b
</code> 
It has two major flaws:\\ 
it forces all the submodules to be imported, even if you're only using one or two, and you still can't look at any of the submodules and quickly see their dependencies at the top, you have to go sifting through functions.
==== Don'ts ====
* you shouldn't be using the 4th syntax, since it only works in python2 and runs the risk of clashing with other 3rd party modules.

===== Classes =====

When defining a class who does not inherit give it ''object'' instead of nothing.

Not every method should be accessed outside of the class.
Therefore it is good practice to use private and protected methods. 

Try to make classes as thin as possible.

Functions which are not under the responsibilities of a class but their logic is used by the class should not be defined in the class. (RDD Responsibility Driven Development) 

Just put the function in the surrounding scope.

<code python>
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

</code>

===== Functions/Methods =====

==== Closures ====

User-defined functions incur a layer of overhead that hurts performance.

Therefore, lambdas and nested functions should be avoided (unless it promotes readability).

==== Recursion ====

Python does not optimize for recursion.

The for loop is, effectively, the same abstraction that recursion provides in functional programming languages.

<code python>
    # Bad
    arr = map(lambda unit: unit.wanted_field, a_list)
    
    # The best
    arr = [unit.field for unit in a_list]
</code>

==== Argument Definition / Function signatures ====

Your functions should have up to 3 parameters (not including self and kind).

Use splat args (*args) and double splat keyword args (%%**%%kwargs) in method definitions to reduce arguments. 

Functions should not receive parameters that they are just suppose to pass to another function. 

If you see a parameter untouched being passed to a few functions, try to put that data into an object or closure who will be passed instead.

<code python>
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
</code>

==== Parameter Passing ====

It is best to be explicit when passing arguments.

Especially boolean options.

This adds readability.

<code python>
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
</code>

==== Dynamic Parameter Expansion ====

You can pass a dynamic list of parameters by using double splat syntax.


===== Exceptions =====


Avoid catching the Exception class. 

<code python>
# bad
try:
  # an exception occurs here
except Exception:
  # exception handling

# good
try:
  # an exception occurs here
except ValueError:
  # exception handling

# acceptable
try:
  # an exception occurs here
except:
  # exception handling
  
</code>

==== EAFP ====

**EAFP** = Easier to ask forgiveness than permission.

As seen in conditions section, exception catching is preferred over checking an object before using.

Python's performance does not suffer when exception handling.

Be careful not to abuse this rule in your flow control.

===== Regular Expressions =====

Use raw strings (''r'.+' '') for regex values.

Raw strings preserve escape characters.

This will save you from unexpected behavior.

Avoid ''*'', use ''+''. 
Bugs are caused because ''*'' also allows for zero occurrences.

Complex regex should be compiled before using.
the resulting variable's name should clearly define it's purpose.

<code python>
# Bad
result = re.match(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$', '31-11-1985')

# Good
valid_date_pattern = re.compile(r'(?:3[01])|(?:[0-2]\d)-(?:1[0-2])|(?:0\d)-\d{4}$')
valid_date_pattern.match('31-11-1985')
</code>

==== Usage ====

Regex is not preferable in python.

Try to avoid using.

There are plenty of tools which are preferred (i.e. ''in'', ''startswith'', ''endswith'', ''isdigit'', ''istitle'', and more).

<code python>
# Bad
is_bob_pattern = re.compile(r'^Bob')
is_bob_pattern.match(name)

has_a_qu_pattern = re.compile(r'qu', re.I)
has_a_qu_pattern.match(word)

# Good
name.startswith('Bob')

'qu' in word.lowercase()
</code>


