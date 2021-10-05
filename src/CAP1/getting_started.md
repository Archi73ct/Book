# Getting Started
Reverse engineering is the practice of working from a limited set of information,
back into some original or equivalent form of _something_.\
As an exampe, take a moment to think about how you would go about figuring out
how a car works, just from looking at an already assembled car?\
Where would you start?\
How would you approach it?\

Reverse engineering is a cool hobby and a great skillset to have.
But how do you get started with picking things apart?
The answer is to just trow yourself at it.\

### Examle
Here is a Hello World example:
```Python
from os import system
system(''.join([chr(x^0x44) for x in [33, 39, 44, 43, 100, 44, 33, 40, 40, 43, 100, 51, 43, 54, 40, 32]]))
```
What does the above program do, and how?

There are several ways of figuring out the answers to the above questions.
This will illustrate the two different ways of approaching reverse engineering.
_Dynamic_ and _static_ analysis are the names of these two techniques.\
You could have solved the above in two ways:
- Run the code in python and observe the output (dynamic)
- Understand the code and what it does, to figure out what gets executed (static)

This book will have many, many examples and I encourage you to try them out
yourself. Here are a few possible solutions to the above:

#### Running the code
Executing the code in a python shell will produce the following output:
```
>>> from os import system
>>> system(''.join([chr(x^0x44) for x in [33, 39, 44, 43, 100, 44, 33, 40, 40, 43, 100, 51, 43, 54, 40, 32]]))
hello world
0
```

#### Running parts of the code
When dealing with interpreted languages like python, we can execute just the
interesting parts of the code. In this case we can see that system is being 
called with some parameter, by evaluating just the parameter, we can avoid 
accidentially executing something bad on our machine:
```
>>> ''.join([chr(x^0x44) for x in [33, 39, 44, 43, 100, 44, 33, 40, 40, 43, 100, 51, 43, 54, 40, 32]])
'echo hello world'
```
So the parameter passed to the `system` function is "echo hello world".

#### Statically
In this context, it is much like the above, except we need to understand what
the code is doing.
The inner part of the `system` call is the interesting part.
It will concatenate a string from a list using `''.join()`.
The list is build with the list expression syntax, and can also be written like:
```Python
for x in [33, 39, 44, 43, 100, 44, 33, 40, 40, 43, 100, 51, 43, 54, 40, 32]:
    c = x^0x44
    print(chr(c), end="")
```
It will xor every number in the list with 0x44. So implementing the same in
python (as above) or any other language, we can see the result.\

*Very cool!* Now you have gotten a feel for it.


