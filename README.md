# Python Notes

My notes while learning to program Python.
Written in short form to help jog my memory in the future.

---
## Feature overview by version

Feature | Python 3 | Python 2
-|-|-
Fn Parenthesis | Required | Optional

## Supported features:
- Short-circuit evaluation
- List comprehensions

---
## Hello world
```python
print("hello world") # script syntax (like Bash script)
```

---
## Main function
This code is typically referred to as the "main block" or the "main function." It is used to indicate that the code in this block should only be executed if the script is run directly, rather than imported as a module into another script.

This structure is only used by scripts that import other scripts, and are meant to be invoked on the command line.

```python
def main(): # program syntax (for larger apps)
  print("starting up...")

if __name__ == "__main__": # only true if this file is run directly (ie. not a module include)
  main()
```

---
## Meaningful indentation
There are no braces, just indentation. Kind of like HAML or CoffeeScript. One of my favorite features.

---
## Comments

Beginning with pound (`#`)

```python
# example comment
```

----
## None value

C++ has `NULL`  
Golang has `nil`  
Python has `None` (object)
It only equals itself; Unlike Javascript, no other value is loosely equal to None.

```python
print(None) # None
print(None == 0) # False
print(None == None) # True, but DON'T do this! `__eq__` operator (`==`) can be overridden, as in C++
print(None is None) # True; use this identity operator (`is`), it cannot be overridden.
```

---
## Dynamically-typed

Python is a dynamically-typed language, which means that the type of a variable is determined at runtime rather than at compile-time. This means that a variable can change its type during the execution of the program.

However, as in Golang, the Python addition/concatenation operator requires both operands to be the same type.

Language|Inference|Concat(T1,T2)
-|-|-
Javascript | run-time | implicit cast
Python | run-time | explicit cast
Golang | compile-time | explicit cast

```python
print("string" + 1) # type error
print("string" + str(1)) # "string1"
print("string", 1) # "string 1"; the easy way, when debugging
```

If you want to specify a type on a variable, this can be done with casting.

```python
x = int("1") # 1
y = float("2.8") # 2.8
z = str(3) # "3"
```

---
## Data types
5 major types: numbers, strings, booleans, sequences, dictionaries.

```python
myint = 5
myfloat = 13.2
mystr = "this is a string"
mybool = True # NOTICE: boolean literals are Capitalized
mylist = [0, 1, "two", 3.2, False] # sequence type; lists
mytuple = (0, 1, 2) # tuple; these are immutable, cannot be changed after declaration
mydict = {"one" : 1, "two" : 2} # dictionaries; map/key-value
```

Furthermore, `print()` can serialize all of these types to human-readable version. 

---
## Variable declaration

Unlike Golang and Javascript, there is no `var` keyword or `:=` operator to differentiate variable declaration from assignment. Therefore, re-declaration of a variable name is allowed; it's effectively just re-assignment.

```python
a = 1 # valid; declaration
a = 2 # valid; re-assignment
```

As in Golang, variables declared outside of a fn are `global` scope, while inside fn is `local` scope.

```python
glo = 1 # GC w/ proc end
def fn1():
  loc = 2 # GC w/ fn end
```

Unlike Golang/C++, you cannot reference global namespace from inside a fn local scope, unless you explicitly declare which vars from global ns you want to use.

```python
a = 1
b = 2
def fn1():
  global a
  a = 3 # modifies global `a`
  b = 4 # shadows; global `b` is unmodified
```

---
## Slices

Slice syntax works on sequences (incl. strings and tuples), as in Golang.

syntax:
```
var[start:end:step]
```

example:

```python
list = ["a","b","c","d"]
print(list[0]) # ["a"]
print(list[0:2]) # ["a", "b"]
print(list[0:2:1]) # ["a", "b"]
print(list[0:3:2]) # ["a", "c"]
str = "hello"
print(str[1:3]) # "el"
tup = (1,2,3)
print(tup[1]) # 2
```

---
## Dictionaries

Uses square brace syntax, as in Javascript.

```python
m = { "a": 1, "b": 2 }
print(m["a"]) # 1
```

---
## Loops

There are only two kinds of loops in Python: `for`, `while`  
There are three control keywords: `continue`, `break`, and `pass` (rare; for empty loops)

To iterate number range `for i in range(start,[end],[step=1]):`  
To iterate sequence values `for v in mylist:`  
To iterate sequence index,values `for i,v in enumerate(mylist):`  
To iterate map key,values `for k,v in mydict.items():`  
To iterate map keys `for k in mydict.keys():`  
To iterate map values `for v in mydict.values():`  

```python
for i in range(3): # 0..3 (zero-indexed; NOT inclusive of 3!)
  print(i) # 0,1,2
else: # NOTICE: always executed! unless `break` is used. should be called `finally:`
  print("nothing to do")
```

```python
x = 0
while (x < 5):
  print(x)
  x = x + 1
```

```python
while True:
  pass # infinite loop. congratulations!
```

---
## If...Else conditional statements

```python
if False:
  print(1)
elif True:
  print(2) # 2
else:
  print(3)
```

---
## Ternary operator

A little wonky.

Javascript `a ? b : c`  
Lua `a and b or c`  
Python `b if a else c`  

```python
cond = False
str = "a" if cond else "b"
print(str) # b
```

---
## Switch conditional statement

Until recently, this is commonly accomplished using only `if...elif...else`.  
Now it's a little more like the pattern-matching style of Haxe.

```python
v = "z"
match v: # since Python 3.10
  case "x":
    result = 1
  case "y":
    result = 2
  case "z" | "w": # "z" or "w"
    result = 3
  case _: # default
    result = -1
print(result)
```

---
## Functions

In Python, function argument types are not strongly enforced. This means that a function can be called with arguments of different types without raising an error. It also means the type must be validated inside the function.

```python
# @param a - required
# @param b - optional; default value specified
# @param args - varargs; sequence type; must appear last
def fn1(a, b=None, *args):
  if b is None:
    # NOTICE: we have to init b inside the fn, because the default value assignment happens once at fn declaration time, not at fn invocation. So if it were a mutable object type, each subsequent invocation of the fn would be referencing/reusing the same object instance!
    b = []
```

However, there are `type hints` which allow you to specify the expected types using annotations.
Kind of like in Flow JS inline-comment syntax `/*:type*/`, because even though you now have access to the type name as a variable in the local scope, you still have to enforce the type check manually from inside the function.

```python
def greet(name: str) -> str: # since Python 3.5
    assert isinstance(name, str), f"Expected a string but got {type(name)}"
    print("Hello, " + name)

greet("Bob")
```

---
## Classes

A lot like Javascript.

```python
class Animal(): # object
  def __init__(self, sound): # constructor
    # NOTE: `self` is a naming convention; not enforced
    self.sound = sound # instance field

class Bat(Animal): # Bat extends Animal
  def __init__(self, sound, wingSize):
    super().__init__(sound)
    self.wingSize = wingSize

  def emitSound(self,a,b):
    print(self.sound)

class Cat(Animal): # Cat extends Animal
  def __init__(self, sound, furColor):
    super().__init__(sound)
    self.furColor = furColor

  def emitSound(self,d,e): # NOTICE: arg count must match other subclass definitions!
    print(self.sound)

pet1 = Bat("echolocation", 7)
pet2 = Cat("meow", "tabby")

print(pet1.wingSize) # 7
print(pet2.furColor) # tabby
pet1.emitSound(1,2) # echolocation
pet2.emitSound(3,4) # meow
```

---
## Exception handling

Python uses `try...except` and `raise`

```python
try:
  x = 10 / 0 # divide by zero error
  raise ValueError("hell") # throw
except ZeroDivisionError as e:
  print("oops!")
except ValueError as e:
  print("doh!")
except: # catch everything else
  print("eek!")
```

You can declare your own exceptions.

```python
class InvalidAgeException(Exception):
  "Raised when the input value is less than 18" # a docstring; describes class as whole
  pass # indicates empty class
```

You can chain exceptions, which preserves backtraces.

```python
raise RuntimeError('specific message') from error # since Python 3
```

Avoid anti-patterns:

```python
raise Exception("too generic") # difficult to catch, since all exceptions extend this. use specific subtypes.
raise 'string literal' # valid in Python <2.4, but bad practice
```

---
## Docstrings

If we do not assign strings to any variable, they act as comments.

Python docstrings are the string literals that appear right after the definition of a function, method, class, or module. They are used to document our code.

We can access these docstrings using the `__doc__` attribute.

```python
def square(n):
  '''Takes in a number n, returns the square of n'''
  return n**2
print(square.__doc__)
```

Even standard library functions use these to provide documentation.

```python
print(print.__doc__)
```

For more details on how to format documentation and conventions,  
see: https://www.programiz.com/python-programming/docstrings

---
## String interpolation

There are several ways to define strings:
- Single quotes (`'`): find to use for strings, not only chars
- Double quotes (`"`): used to avoid escaping apostrophe/single-quote (`\'`)
- Triple quotes (`'''` or `"""`): use for multi-line expressions

There are also prefixes:
- Raw strings (`r""`): ignores all escape chars
- Byte strings (`b""`): sequence of bytes; for binary data 
- Unicode strings (`u""`): UTF-16 encoding. (Normal strings default to `UTF-8` in Python 3, or `ASCII` in Python 2.)
- F strings/literals (`f""`): used to permit string interpolation with curly braces (`{}`)

There are several ways to interpolate values in your strings:
```python
name, age = "John", 30
print("My name is %s and I am %d years old" % (name, age)) # C sprintf() style
print("My name is {} and I am {} years old".format(name, age)) # String.format()
print(f"My name is {name} and I am {age} years old") # since Python 3.6
```

---
## References

- Python 3 API  
  https://docs.python.org/3/
- Python Package Repository  
  https://pypi.org/

## Notable libraries

- click > argparse
- pytest > unittest
- json
- itertools
- request

---
## Miscellaneous Trivia

### Q: when did python binary rename to "py"?
A: Python 3.3 introduced the new launcher for Windows, which allows the use of "py" as the command instead of python to run the python files. This was included in the release on 2012-09-29 and was added as an option to make it easier to run Python scripts on Windows, as the standard convention on that operating system is to use a .py file extension to indicate that a file is a Python script.
It means that with the new launcher installed, the user is able to run scripts with the "py" command regardless of whether the script has the .py or .pyw extension, and regardless of whether the script was created with Python 2 or Python 3.

### Q: in python language, what is the difference between pip packages and python eggs?
A: In Python, pip and eggs are both used to package and distribute software, but they have some key differences.

pip is the de facto standard package manager for Python. It is used to install and manage Python packages, including those that are part of the Python Standard Library. Pip uses the .tar.gz file format for source distributions and the .whl file format for built distributions. it uses the requirements.txt file to store all the package requirements.

eggs are a legacy package format that was used in the early days of Python. They are similar to pip packages, but they use a different file format (.egg) and come with additional metadata, such as package dependencies. eggs were created to solve some of the limitations of the Python Package Index (PyPI) and setuptools. but now it's not commonly used anymore, as pip is more prevalent and recommended for most use cases.

In summary, pip is the modern package manager for Python, which is widely used and recommended, while eggs are an older package format that is not as commonly used anymore.

### Q: How to invoke PIP with Python Launcher?

```bash
$ py -3 -m pip install requests
$ py -3 -m pip freeze > requirements.txt
$ py -3 -m pip install -r requirements.txt
```

### Q: What's a good Python formatter for use with VSCode to format-on-save?

https://dev.to/adamlombard/how-to-use-the-black-python-code-formatter-in-vscode-3lo0