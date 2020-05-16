# Python Language - Meta Guide

All those tiny details that will eventually come up at a meetup and you need to look like you know your s#$t.

## History

The [Wikipedia article](https://en.wikipedia.org/wiki/History_of_Python) is very good. But this is what you need to know:

Python was created by a Dutch programmer named [Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum).

Python **is an old language**. Yes, I know it looks (and feels) modern, but it's been around for quite some time. Python's first public distribution was in February 1991.

<center>
![](img/820px-Python_logo_1990s.svg.png)
</center>

(Original Python logo from the 90s/early 2000s)

Guido designed Python while he was working at the [CWI](https://en.wikipedia.org/wiki/Centrum_Wiskunde_%26_Informatica) research center in the Netherlands. Python was inspired by another language called [ABC](https://en.wikipedia.org/wiki/ABC_(programming_language)), that was also being developed at CWI. A clear result of the influence of ABC on Python is its usage of indentation, that Guido himself explains in this post: http://python-history.blogspot.com/2011/07/karin-dewar-indentation-and-colon.html

There's more to read about the history of Python. Guido has a few blogs in which he posts stubs from time to time: http://python-history.blogspot.com/


## Python versions

As mentioned in the previous section, Python is an old language. Today's most popular uses of Python are Web Development and Data Science. That didn't even exist at the moment Python was created, back in the 90s. Python has obviously evolved to accommodate to today's needs, and with that evolution came different versions.

Realistically, you only need to know that there are (were?) two major versions:  **Python 2 and Python 3**. Python 2 was the standard version when Python became "popular", I particularly started with Python 2.6 (back in 2008).


Python 2 was a major redesign of Python itself (what used to be Python 1). I don't know many people that used Python 1, so Python 2 was basically the standard. Most of the features we are used to in Python were added in Python 2.

But Python 2 was released in the year 2000, the standard CPU was the Pentium III. You can also guess that many of the decisions made at the time didn't adapt quite well to the modern world.

!!! tip
    Python 2 **is deprecated** and Python 3 is the version you should be using. More about this in the installation and setup section.

So Python 3 was born. It was actually conceived as  Python 3000 or Py3K (I'm not kidding you, terrible names). The discussions about Python 3 started in the year 2006 with [PEP 3000]([PEP 3000 -- Python 3000 | Python.org](https://www.python.org/dev/peps/pep-3000/)) (I'll explain what PEPs are later). Python 3 was again a major redesign of the language, and more importantly, **it was NOT backwards compatible with Python 2**, which didn't appeal to many people. Many important changes were introduced in Python 3, like default unicode support ([PEP 3120]([PEP 3120 -- Using UTF-8 as the default source encoding | Python.org](https://www.python.org/dev/peps/pep-3120/))), print as a function, and others.



Python 3 was developed in parallel with Python 2 regular feature improvements, bugfixes and security patches. The core Python team knew Python 3 was the future and the better option