# Control flow

Python won't give you any unpleasant surprises when it comes to Control Flow. On the contrary, you might find a few interesting features that are not part of other traditional dynamic languages.

Personally, the feature that I miss the most in Python is the lack of "expression oriented control flow", as you can find for example in Scala or F#.

## Indentation

Indentation, as seen before, is what defines the current "active block". Although, as we'll see later in this same chapter, Python control flow structures don't define new "execution frames" or namespaces, differently from other languages as Java or C, for example.

```python
# Not valid Python code, just a demonstration of indentation
if [condition]:
    for [elem in elements]:
        if [another condition]:
            # inside another condition
        else:
            # inside ELSE of another condition
else:
    # Inside else of main if
```

## If statements

An example:

```python
age = int(input('Enter your age: '))
if x > 21:
    print("You can drive and drink")
elif x > 18:
    print("You can only drive")
else:
    print("Sorry. You'll have to wait for all the fun stuff")
```

You can see the use of the `elif` clause, which is just _else if_. Python **does NOT have a switch statement**, so the `elif` clause replaces part of that behavior.

### Ternary operator

The ternary operator is not so commonly used, as it might hurt readability, but it's still useful to know it:

```python
x = int(input("Enter a number: "))
even_or_odd = "even" if (x % 2 == 0) else "odd"
```

A more _"Pythoninc"_ approach would use an explicit if statement:

```python
x = int(input("Enter a number: "))
if x % 2 == 0:
    even_or_odd = "even"
else:
    even_or_odd = "odd"
```

The ternary operator can be chained for even less readable operations, so please try to avoid it:

```python
def teenager_test(age):
    return "drive and drink" if age >= 21 else "only drive" if age >= 18 else "Nothing fun"
```

### Truthy, Falsy

Python's decision to go through an `if` or `else` branch is based on the value of the _expression_ passed. In the boolean operators section we'll discuss in further detail the concepts of truthy/falsy values. For now, what you need to know is that we prefer pythonic expressions that are oriented to truthy/falsy statements, instead of booleans. For example, the following statement is NOT considered Pythonic, as it tries to "force" a boolean:

```python
if user.email_verified is True:
    # body
```

In this case, `user.email_verified` must return a boolean value. It can't return anything else (the datetime of the verification, for example). A more Pythonic approach is just:

```python
if user.email_verified:
    # body
```

The same for collections. A non pythonic approach is:

```python
if len(a_list) > 0:
```

Empty collections are falsy. So we can just rewrite it in this way:

```python
if a_list:
```
> Hint: Remember the issue with the `is` operator. And just use it whenever you want to explicitly check for it.

## For loops

For loops in Python are **NOT** like for loops in other languages. They're what in other languages are called "for each" loops. They let you step over elements from an iterator. Example:

```python
fruits = ['Apple', 'Orange', 'Banana']
for fruit in fruits:
    print(fruit)
```

A traditional for loop in another language looks something like: `for(initialization; condition; step)`, as the following example:

```javascript
// Javascript
for (i = 0; i < 5; i++){
    console.log(i)
}
```

There's not such thing in Python. To recreate the previous example, we need an iterator containing the elements first:

```python
elements = [0, 1, 2, 3, 4]
for elem in elements:
    print(elem)
```

This is obviously not practical, and that's why it's so simple (and crucial to Python) to use iterators and generators. For this particular example, we can use the `range` function:

```python
# prints 0 to 4
for elem in range(5):
    print(elem)
```

The `range` function accepts 3 parameters, `(init, end, step)`:

```python
>>> list(range(2, 10))
[2, 3, 4, 5, 6, 7, 8, 9]
>>> list(range(2, 10, 2))
[2, 4, 6, 8]
>>> list(range(10, 0, -1))
[10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```

### Variable Scope

The first thing you'll notice is that control flow structures don't create namespaces or blocks:

```python
>>> for i in range(5):
>>>     pass
>>> i
4
```

The `i` variable is bound and keeps the value of the last iteration.

The same happens for variables defined "inside" the block:

```python
>>> for _ in range(5):
>>>     a = 9
>>> a
9
```

Again, this indicates that **Python control flow blocks, don't create new execution frames or namespaces**. If you come from a language that actually creates new execution frames, be careful not to clutter the namespace with variables generated inside the control flow blocks.

This might also result in some unpleasant surprises, like in this example inspired by an [interesting discussion in Python's email list](https://mail.python.org/pipermail/python-ideas/2008-October/002109.html):

```python
>>> lst = []
>>> for i in range(3):
>>>     lst.append(lambda: i)

>>> [f() for f in lst]
[2, 2, 2]
```

### List Comprehensions

The last statement of our previous example is a list comprehension: `[f() for f in lst]`. There's more about list comprehensions in the *Functional programming* chapter, but in a glance, this is what you need to know:

A list comprehension works with any iterable, not just lists (should we call it *iterator comprehension*?). It has the following form `[(expression) for (element) in (iterable)]`. Optionally, it can be passed a "filtering" expression at the end. The final result then is: `[(expression) for (element) in (iterable) if (condition)]`. Examples will speak by themselves:


```python
>>> [x**2 for x in range(5)]
[0, 1, 4, 9, 16]

>>> [x**2 for x in range(5) if x > 2]
[9, 16]

>>> [(x, x**2) for x in range(5) if x > 2]
[(3, 9), (4, 16)]
```

Our previous example involving weird scoping of variables can be rewritten using comprehensions in the following way:

```python
>>> lst = [lambda: i for i in range(4)]
>>> [f() for f in lst]
[3, 3, 3, 3]
```

> **Hint**: Always prefer list comprehensions over for loops when possible. List comprehensions are expressive and declarative, compared to for loops which are imperative.

### `break` and `continue`

The `break` clause will immediately break the current for loop:

```python
user_to_find = 'mary@example.com'
for user in users:
    if user == users_to_find:
        print("User found!")
        break
```



* Enumerate
* Unpacking
## Summary

No switch/case.