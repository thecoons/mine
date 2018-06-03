# Effective Python: 59 SPECIFIC WAYS TO WRITE BETTER PYTHON

## Introduction

## Feedbacks

### Item 15 : Know how __closures__ interact with __variable scope__
Say you want to sort a list of numbers but prioritize one group of numbers to come first.
This pattern is useful when you’re rendering a user interface and want important messages
or exceptional events to be displayed before everything else.

```python
def sort_priority(numbers, group):
    found = False # Scope: ‘sort_priority2’

    def helper(x):
        if x in group:
            found = True # Scope: ‘helper’ — Bad!
            return (0, x)
        return (1, x)

    numbers.sort(key=helper)
    return found
```

When you reference a variable in an expression, the Python interpreter will traverse the
scope to resolve the reference in this order:
1. The current function’s scope
2. Any enclosing scopes (like other containing functions)
3. The scope of the module that contains the code (also called the global scope)
4. The built-in scope (that contains functions like len and str)
5. If none of these places have a defined variable with the referenced name, then a
NameError exception is raised.

```python
def sort_priority(numbers, group):
    found = False

    def helper(x):
        nonlocal found
        if x in group:
            found = True
            return (0, x)
        return (1, x)

    numbers.sort(key=helper)
    return found
```

```python
class Sorter(object):
    def __init__(self, group):
        self.group = group
        self.found = False

    def __call__(self, x):
        if x in self.group:
            self.found = True
            return (0, x)
        return (1, x)


sorter = Sorter(group)
numbers.sort(key=sorter)

assert sorter.found is True
```

#### Things to Remember
1. Closure functions can refer to variables from any of the scopes in which they were
defined.
2. By default, closures can’t affect enclosing scopes by assigning variables.
3. In Python 3, use the nonlocal statement to indicate when a closure can modify a
variable in its enclosing scopes.
4. In Python 2, use a mutable value (like a single-item list) to work around the lack of
the nonlocal statement.
5. Avoid using nonlocal statements for anything beyond simple functions.

#### Things to Notice
1. Python has specific rules for comparing tuples. It first compares items in index zero,
then index one, then index two, and so on. This is why the return value from the
helper closure causes the sort order to have two distinct groups.
2. Functions are first-class objects in Python, meaning you can refer to them directly,
assign them to variables, pass them as arguments to other functions, compare them
in expressions and if statements, etc. This is how the sort method can accept a
closure function as the key argument.
3. The puprose of **\_\_call\_\_** is perfect to implement this behaviour when you want use a class to be more readble or something else



## Latex reference
@book{book:1311629,
   title =     {Effective Python: 59 SPECIFIC WAYS TO WRITE BETTER PYTHON},
   author =    {Brett Slatkin},
   publisher = {Addison-Wesley},
   isbn =      {9780134034287},
   year =      {2015},
   series =    {},
   edition =   {},
   volume =    {},
   url =       {http://gen.lib.rus.ec/book/index.php?md5=52270435FF6F832BB4A3DF90ED1550C8}}