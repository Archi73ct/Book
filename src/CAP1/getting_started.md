# Getting Started
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
