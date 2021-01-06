# Language Basics

There's not much you need to know about Python basics to get started, aside from:

## Indentation

Python uses indentation to define blocks, and each "level" of indentation is separated by the colon sing (`:`). An example explains it better:

```python
for user in users:
    if user.is_active:
        try:
            send_email(user)
        except EmailException:
            logging.error("An error occurred")
    else:
        logging.debug("Skipping, inactive user")
```

There's an amusing easter egg built into the language involving braces:

```python
>>> from __future__ import braces
  File "<stdin>", line 1
SyntaxError: not a chance
```

Each statement in Python is determined with a new line, as you can see from the example above. Optionally, you can use a semicolon (`;`) to separate multiple statements in the same line, but this is rarely used:

```python
>>> import random; random.randint(0, 99)
30
```

## Tabs vs spaces: code formatting

Do NOT use tabs. Use 4 spaces for indentation level. It's recommended by [PEP 8](https://www.python.org/dev/peps/pep-0008/) and it's a standard in the industry. Make sure you set your editor with the right configuration.

## Comments

Python has only one type of comment, a single-line comment. To create a comment, preface the line with the pound character (`#`). Example:

```python
# This is a comment
# This too
print("This # is not#")
```

Comments are stripped out in the process of interpretation (when the interpreter compiles the source into bytecode) so it's totally safe to add as many comments as you want (sorry, you can't use "size" as an excuse not to comment your code).

There are no multi-line comments in Python. There's a hack though, an alternative "solution", which involves using multiline strings (a pair of triple quotes `"""` or `'''`):

```python
"""
This code
has no effect

1/0 doesn't break for example
"""
```

But again, these are not actual comments and they'll be evaluated by your Python interpreter. They might have unintended consequences in some situations (like in function docs) and they just don't look like comments. Your editor should include a simple way to comment/uncomment pieces of code using the pound char `#`.

!!! warning
    Try to avoid multi-line strings as comments. Instead, put some time in configuring your editor to comment out block of codes using a simple key combination, like `cmd + /`.

## Python program evaluation

Python parses a program from top to bottom, and processes definitions as it finds them. This might have some unintended consequences. For example, the following code will raise an exception:

```python
say_hello()
def say_hello():
    print("Hello world")
```

The exception is a `NameError`, as we're trying to invoke a function `say_hello` and it doesn't exit (yet). It will _start existing_ once the function block is evaluated.

## Variables and names

Variables are set by just assigning them to a value, there's no need to "declare" them. Assignment is performed with the equals sign (`=`).

```python
x = 2
name = "String"
email = 'hello@example.com'
```

!!! Note:
    There's no difference between single and double quoted strings. See the chapter about strings for more.

Variable names don't have a length restriction. Valid characters are uppercase and lowercase letters (`A-Z`, `a-z`), digits (`0-9`), and the underscore character (`_`). Although digits can't be at the start of the variable name: `99name` is invalid and `_99name` is valid.

You can delete a variable using the `del` keyword, but this doesn't mean the underlying value is deleted and the memory is free. Remember Python's Garbage Collector and reference counting:

```python
>>> class Car:
>>>     pass

>>> x = Car()
>>> x
<__main__.Car object at 0x10fc52850>
>>> y = x
>>> y
<__main__.Car object at 0x10fc52850>
>>> del x
>>> y
<__main__.Car object at 0x10fc52850>
>>> x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```

## Object identity

In our previous example you'll see a hex number associated with the `Car` object. In Python, each object created has a unique identifier that you can check with the `id` function:

```python
>>> x = 1
>>> id(x)
4363573440
>>> id("Hello World")
4368164144
```

By checking IDs you'll find out that some objects in python are "interned", or cached for simplicity. This is evident for integers:
```python
>>> x = 1
>>> y = 1
>>> z = 4 - 3
>>> id(x)
4363573440
>>> id(y)
4363573440
>>> id(z)
4363573440
```

This means that there's only 1 instance of the object `1` created and it's always reused. It happens the same thing for strings:

```python
>>> s1 = "hello"
>>> s2 = "hello"
>>> id(s1)
4368226800
>>> id(s2)
4368226800
>>> id("hello")
4368226800
```

But you should **be careful** with object interning, because it's **interpreter dependant** (I have tried this only in CPython 3.8) and it's just done for a handful of objects. For example, for integers, it only applies for integers between `-5` and `256`. From the [C-API docs](https://docs.python.org/3/c-api/long.html):

> The current implementation keeps an array of integer objects for all integers between -5 and 256, when you create an int in that range you actually just get back a reference to the existing object. So it should be possible to change the value of 1. I suspect the behaviour of Python in this case is undefined. :-)

In the following example you see that even though both objects represent the same number, they're actually to difference instances:

```python
>>> x = 257
>>> y = 257
>>> id(x)
4537070032
>>> id(y)
4537070064
```

## The `is` operator

We have an entire chapter discussing operators, but it's important to discuss the `is` operator early, as it is a usual source of confusion. The `is` operator, applied in the form `obj1 is obj2`, checks if `obj1` and `obj2` are **the same object**. It's equivalent to `id(obj1) == id(obj2)`. Which, given the previously discussed "implementation specific" behavior of object interning, might throw "unexpected" results:

```python
>>> x = 1
>>> y = 1
>>> x is y
True
# The following line might result surprising for new Pythoners:
>>> x = 257
>>> y = 257
>>> x is y
False
```

In reality, this is completely expected behavior. The correct operator to use to check equality is `==`. As a rule of thumb, try to avoid the `is` operator. There could be an exception for explicit boolean checks (`x is True`), but that'd mean that you want to check that `x` is literally `True`, when in reality the desire is to check if `x` is _truthy_. More about all of this in the Data Types and Operators chapters.


## Python Bytecode

Similarly to Java, Python will transform your source code into bytecode (an intermediary step) before actually running it. The process of running `python main.py` is roughly:

* The interpreter will load the `main.py` script and generate the intermediate _bytecode_
* The Python runtime (also called virtual machine) will now run the generated _bytecode_.

There are two main advantages of the bytecode format. First, Python can optimize the bytecode without requiring you to make changes to your scripts (see the [peephole optimizer](https://github.com/python/cpython/blob/e682b26a6bc6d3db1a44d82db09d26224e82ccb5/Python/peephole.c)).

Secondly, the bytecode generated can be cached as long as modules are not modified. Python stores cached bytecode in files with the extension `*.pyc`. Starting with Python 3.2 (see [PEP 3147](https://www.python.org/dev/peps/pep-3147/)), all bytecode cached files are located in a special directory `__pycache__`. Back when we were using Python 2, these files would be at the same location as the actual Python module and it was very annoying. If you don't want Python to generate these cached bytecode files, pass the `-B` flag to the interpreter or set the `PYTHONDONTWRITEBYTECODE` to anything, although this is not recommended (caching is good).

The [`dis` module](https://docs.python.org/3/library/dis.html) includes functions for working with Python bytecode by disassembling it into a more human-readable form. For example, let's disassemble the statement `x += 1`:

```python
x = 0
x += 1
```

If you put that in a module (`my_dis_module.py`) and run dis from the command line `python -m dis my_dis_module.py`, we'll get:

```
  1           0 LOAD_CONST               0 (0)
              2 STORE_NAME               0 (x)

  2           4 LOAD_NAME                0 (x)
              6 LOAD_CONST               1 (1)
              8 INPLACE_ADD
             10 STORE_NAME               0 (x)
             12 LOAD_CONST               2 (None)
             14 RETURN_VALUE
```

In the first chunk corresponds to `x = 0`. The CPython interpreter is stack based, which means that the values/constants are allocated first. It follows then by referencing the variable `x` to that previously stored value. The second chunk shows the operation `INPLACE_ADD`, which is the bytecode operation generated by the `+=` operator.