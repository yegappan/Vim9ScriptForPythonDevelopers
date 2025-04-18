# Vim9 Script for Python Developers

A guide to Vim9script development for Python developers. This guide presents sample code for various expressions, statements, functions, and programming constructs, shown in both Python and Vim9script. It is not intended as a tutorial on Vim script development, and assumes the reader is already familiar with Python programming.

For an introduction to Vim9 Script development, refer to [vim9.txt](https://vimhelp.org/vim9.txt.html), [vim9class.txt](https://vimhelp.org/vim9class.txt.html), [usr_41.txt](https://vimhelp.org/usr_41.txt.html) and [eval.txt](https://vimhelp.org/eval.txt.html).

This guide focuses only on programming constructs that are common to both Python and Vim9. Constructs unique to Vim - such as [autocommands](https://vimhelp.org/autocmd.txt.html#autocommands), [key-mapping](https://vimhelp.org/map.txt.html#key-mapping), [abbreviations](https://vimhelp.org/map.txt.html#abbreviations), [user-commands](https://vimhelp.org/map.txt.html#user-commands) and [plugins](https://vimhelp.org/usr_05.txt.html#plugin) - are not covered in this guide.

The Vim9 script features are only available starting from Vim 9.0.

The Github repository for this gist is available at: https://github.com/yegappan/Vim9ScriptForPythonDevelopers.

*Note*: The first command in a Vim9script file should be the [:vim9script](https://vimhelp.org/repeat.txt.html#:vim9script) command.  However, for simplicity, this guide omits it from the examples.

------------------------------------------------------------------------------

## Table of Contents
* [Literals](#literals)
* [Variables](#variables)
* [Operators](#operators)
* [String](#string)
* [List](#list)
* [Tuple](#tuple)
* [Dictionary](#dictionary)
* [Enum](#enum)
* [If Statement](#if-statement)
* [For Loop](#for-loop)
* [While Loop](#while-loop)
* [Comment](#comment)
* [Function](#function)
* [Lambda Function](#lambda-function)
* [Partial Function](#partial-function)
* [Closure](#closure)
* [Class](#class)
* [Exception Handling](#exception-handling)
* [Line Continuation](#line-continuation)
* [File Operations](#file-operations)
* [Directory Operations](#directory-operations)
* [Random Numbers](#random-numbers)
* [Mathematical Functions](#mathematical-functions)
* [Date/Time Functions](#datetime-functions)
* [External Commands](#external-commands)
* [User Input/Output](#user-inputoutput)
* [Environment Variables](#environment-variables)
* [Command Line Arguments](#command-line-arguments)
* [Regular Expressions](#regular-expressions)
* [Binary Data](#binary-data)
* [Timers](#timers)
* [JSON encoder and decoder](#json-encoder-and-decoder)
* [Network Sockets](#network-sockets)
* [Background Processes](#background-processes)
* [Unit Tests](#unit-tests)

------------------------------------------------------------------------------

## Literals

Type| Python | Vim9Script
----|--------|---------
integer| `-1, 0, 5` | `-1, 0, 5`
binary| `0b1011` | `0b1011`
octal| `0o477` | `0477`
hexadecimal| `0x1AE` | `0x1AE`
float| `3.14, -1.5e2` | `3.14, -1.5e2`
string| `"hello", 'world'` | `"hello", 'world'`
boolean| `True, False` | `true, false`
list| `[], [5, 9], ['a', 'b']` | `[], [5, 9], ['a', 'b']`
tuple| `(), (1,), (5, 9), ('a', 'b')` | `(), (1,), (5, 9), ('a', 'b')`
dict| `{}, {'idx' : 2, 'name' : 'abc'}` | `{}, {idx: 2, name: 'abc'}`
special| `None` | `none, null`

*Help:* [expr-number](https://vimhelp.org/eval.txt.html#expr-number), [string](https://vimhelp.org/eval.txt.html#string), [Boolean](https://vimhelp.org/eval.txt.html#Boolean), [list](https://vimhelp.org/eval.txt.html#list), [tuple](https://vimhelp.org/eval.txt.html#tuple), [dict](https://vimhelp.org/eval.txt.html#dict), [v:none](https://vimhelp.org/eval.txt.html#v:none), [v:null](https://vimhelp.org/eval.txt.html#v:null)

------------------------------------------------------------------------------

## Variables

### Creating a Variable
**Python:**
```python
i = 10
pi = 3.1415
str = "Hello"
a, b, s = 10, 20, "sky"
```

**Vim9Script:**
```vim
var i = 10
var pi = 3.1415
var str = "Hello"
var [a, b, s] = [10, 20, "sky"]
```

You can also create the variables with a specific type declaration:
```vim
var i: number = 10
var pi: float = 3.1415
var str: string = "Hello"
var [a: number, b: number, s: string] = [10, 20, "sky"]
```

In Vim9script, strict type checking is recommended, so specifying the type of each variable is advised. However, for simplicity, this guide omits explicit type declarations and relies on inferred types instead.

*Help:* [variables](https://vimhelp.org/eval.txt.html#variables)

### Deleting a Variable
**Python:**
```python
del str
```

**Vim9Script:**
```vim
Variables are garbage collected in Vim9 and cannot be deleted.
```

*Help:* [:unlet](https://vimhelp.org/eval.txt.html#%3Aunlet)

### Here Document
Assigning multi-line values to a variable.

**Python:**
```python
import textwrap
i = textwrap.dedent("""
    one
    two three
      four
    five
""".lstrip('\n')).splitlines()
# i == ['one', 'two three', '  four', 'five']
```

**Vim9Script:**
```vim
var i =<< trim END
  one
  two three
    four
  five
END
# i == ['one', 'two three', '  four', 'five']
```

*Help:* [:let-heredoc](https://vimhelp.org/eval.txt.html#%3Alet-heredoc)

### Types

Type|Python|Vim9Script
----|------|---------
Number| `num = 10` | `var num: number = 10`
Float| `f = 3.4` | `var f: float = 3.4`
Booelan| `done = True` | `var done: bool = true`
String| `str = "green"` | `var str: string = "green"`
List| `l = [1, 2, 3]` | `var l: list<number> = [1, 2, 3]`
Tuple| `t = (1, 2, 3)` | `var t: tuple<number> = (1, 2, 3)`
Dictionary| `d = {'a' : 5, 'b' : 6}` | `var d: dict<number> = {a: 5, b: 6}`

*Help:* [variables](https://vimhelp.org/eval.txt.html#variables)

### Type Conversion

Conversion|Python|Vim9Script
----------|------|---------
Number to Float| `float(n)` | `floor(n)`
Float to Number| `int(f)` | `float2nr(f)`
Number to String| `str(n)` | `string(n)`
String to Number| `int(str)` | `str2nr(str)`
Float to String| `str(f)` | `string(f)`
String to Float| `float(str)` | `str2float(str)`
List to String| `str(l)` | `string(l)`
String to List| `eval(str)` | `eval(str)`
List to Tuple| `tuple(l)` | `list2tuple(l)`
Tuple to List| `list(t)` | `tuple2list(t)`
Tuple to String| `str(t)` | `string(t)`
String to Tuple| `eval(str)` | `eval(str)`
Dict to String| `str(d)` | `string(d)`
String to Dict| `eval(str)` | `eval(str)`

*Help:* [string()](https://vimhelp.org/builtin.txt.html#string%28%29), [str2nr()](https://vimhelp.org/builtin.txt.html#str2nr%28%29), [str2float()](https://vimhelp.org/builtin.txt.html#str2float%28%29), [float2nr()](https://vimhelp.org/builtin.txt.html#float2nr%28%29), [floor()](https://vimhelp.org/builtin.txt.html#floor%28%29), [eval()](https://vimhelp.org/builtin.txt.html#eval%28%29), [list2tuple()](https://vimhelp.org/builtin.txt.html#list2tuple%28%29), [tuple2list()](https://vimhelp.org/builtin.txt.html#tuple2list%28%29)

### Type Check

Type|Python|Vim9Script
----|------|---------
Number| `isintance(x, int)` | `type(x) is v:t_number`
String| `isinstance(x, str)` | `type(x) is v:t_string`
List| `isintance(x, list)` | `type(x) is v:t_list`
Tuple| `isintance(x, tuple)` | `type(x) is v:t_tuple`
Dictionary| `isintance(x, dict)` | `type(x) is v:t_dict`
Float| `isintance(x, float)` | `type(x) is v:t_float`
Boolean| `isinstance(x, bool)` | `type(x) is v:t_bool`

*Help:* [type()](https://vimhelp.org/builtin.txt.html#type%28%29)

### Variable Namespaces

All the variables in a Vim9Script have a scope. The default scope for variables and functions is script-local. The scope is specified by prefixing the variable name with one of the prefix strings shown in the table below. If a variable name is used without a scope prefix, then inside a function the function local scope is used. Otherwise the script-local scope is used.

Scope Prefix|Description
-----|-----------
g: | global
v: | internal
b: | buffer local
w: | window local
t: | tab local

*Help:* [global-variable](https://vimhelp.org/eval.txt.html#global-variable), [vim-variable](https://vimhelp.org/eval.txt.html#vim-variable), [buffer-variable](https://vimhelp.org/eval.txt.html#buffer-variable), [window-variable](https://vimhelp.org/eval.txt.html#window-variable), [tabpage-variable](https://vimhelp.org/eval.txt.html#tabpage-variable)

------------------------------------------------------------------------------

## Operators

### Arithmetic Operators

What|Python|Vim9Script
----|------|---------
addition| `a = 10 + 20` | `a = 10 + 20`
subtraction| `a = 30 - 10` | `a = 30 - 10`
multiplication| `a = 3 * 5` | `a = 3 * 5`
division| `a = 22 / 7` | `a = 22 / 7`
modulus| `a = 10 % 3` | `a = 10 % 3`
exponentiation| `a = 2 ** 3` | `a = float2nr(pow(2, 3))`
floor division| `a = 10 // 3` | `a = floor(10 / 3.0)`

*Help:* [expr5](https://vimhelp.org/eval.txt.html#expr5)

### Assignment Operators

What|Python|Vim9Script
----|------|---------
addition| `a += 1` | `a += 1`
subtraction| `a -= 2` | `a -= 2`
multiplication| `a *= 4` | `a *= 4`
division| `a /= 2` | `a /= 2`
modulus| `a %= 2` | `a %= 2`

*Help:* [:let+=](https://vimhelp.org/eval.txt.html#%3alet+=)

### Comparison Operators

What|Python|Vim9Script
----|------|---------
equal to| `a == b` | `a == b`
not equal to| `a != b` | `a != b`
greater than| `a > b` | `a > b`
less than| `a < b` | `a < b`
greater than or equal to| `a >= b` | `a >= b`
less than or equal to| `a <= b` | `a <= b`

*Help:* [expr4](https://vimhelp.org/eval.txt.html#expr4)

### Logical Operators

What|Python|Vim9Script
----|------|---------
logical and| `x and y` | `x && y`
logical or| `x or y` | `x \|\| y`
logical not| `not x` | `!x`

*Help:* [expr2](https://vimhelp.org/eval.txt.html#expr2)

### Bitwise Operators

What|Python|Vim9Script
----|------|---------
bitwise AND| `c = a & b` | `c = and(a, b)`
bitwise OR| `c = a \| b` | `c = or(a, b)`
bitwise NOT| `c = ~a` | `c = invert(a)`
bitwise XOR| `c = a ^ b` | `c = xor(a, b)`
left shift| `c = a << b` | `c = a << b`
right shift| `c = a >> b` | `c = a >> b`

*Help:* [and()](https://vimhelp.org/builtin.txt.html#and%28%29), [or()](https://vimhelp.org/builtin.txt.html#or%28%29), [invert()](https://vimhelp.org/builtin.txt.html#invert%28%29), [xor()](https://vimhelp.org/builtin.txt.html#xor%28%29)

### Identity Operators

What|Python|Vim9Script
----|------|---------
is| `x is y` | `x is y`
is not| `x is not y` | `x isnot y`

*Help:* [expr-is](https://vimhelp.org/eval.txt.html#expr-is), [expr-isnot](https://vimhelp.org/eval.txt.html#expr-isnot)

------------------------------------------------------------------------------

## String

Vim has functions for dealing with both multibyte and non-multibyte strings. Some functions deal with bytes and some functions deal with characters. Refer to the reference guide for the various functions to get the detailed information.

### Raw string

**Python:**
```python
s1 = r"one\ntwo\n"
s2 = "one\ntwo\n"
```

**Vim9Script:**
```vim
var s1 = 'one\ntwo\n'
var s2 = "one\ntwo\n"
```

*Help:* [literal-string](https://vimhelp.org/eval.txt.html#literal-string), [string](https://vimhelp.org/eval.txt.html#string)

### Finding string length

**Python:**
```python
n = len("Hello World")
```

**Vim9Script:**
```vim
# count the number of bytes in a string
var n = len("Hello World")
# count the number of bytes in a string
n = strlen("Hello World")
# count the number of characters in a string
n = strcharlen("Hello World")
# count the number of characters in a string
n = strwidth("Hello World")
```

*Help:* [len()](https://vimhelp.org/builtin.txt.html#len%28%29), [strlen()](https://vimhelp.org/builtin.txt.html#strlen%28%29), [strcharlen()](https://vimhelp.org/builtin.txt.html#strcharlen%28%29), [strwidth()](https://vimhelp.org/builtin.txt.html#strwidth%28%29), [strdisplaywidth()](https://vimhelp.org/builtin.txt.html#strdisplaywidth%28%29)

### String Concatenation

**Python:**
```python
str1 = "blue"
str2 = "sky"
s = str1 + str2
```

**Vim9Script:**
```vim
var str1 = "blue"
var str2 = "sky"
var s = str1 .. str2
```

### String Comparison

**Python:**
```python
# compare strings matching case
str1 = "blue"
str2 = "blue"
str3 = "sky"
if str1 == str2:
  print("str1 and str2 are same")
if str1 is str2:
  print("str1 and str2 are same")
if str1 != str3:
  print("str1 and str3 are not same")
if str1 is not str3:
  print("str1 and str3 are not same")

# compare strings ignoring case
str1 = "Blue"
str2 = "BLUE"
str3 = "sky"
if str1.lower() == str2.lower():
  print("str1 and str2 are same")
if str1.lower() != str3.lower():
  print("str1 and str3 are not same")
```

**Vim9Script:**
```vim
# compare strings matching case
var str1 = "blue"
var str2 = "blue"
var str3 = "sky"
if str1 == str2
  echo "str1 and str2 are same"
endif
if str1 != str3
  echo "str1 and str3 are not same"
endif

# compare strings ignoring case
str1 = "Blue"
str2 = "BLUE"
str3 = "sky"
if str1 ==? str2
  echo "str1 and str2 are same"
endif
if str1 !=? str3
  echo "str1 and str3 are not same"
endif
```

*Help:* [expr4](https://vimhelp.org/eval.txt.html#expr4)

### Getting a substring

In Python, when using an index range to specify a sequence of characters in a string, the character at the end index is not included. In Vim, the character at the end index is included.

**Python:**
```python
str = "HelloWorld"
sub = str[2 : 5]
sub = str[-3 : ]
sub = str[2 : -3]
```

**Vim9Script:**
```vim
var str = "HelloWorld"
# use character index range
var sub = str[2 : 4]
# use a negative character range
sub = str[-3 : ]
sub = str[2 : -3]
# use byte index and length
echo strpart(str, 2, 3)
# use character index and length
echo strcharpart(str, 2, 3)
# use the start and end character indexes
echo slice(str, 2, 3)
# exclude the last character
echo slice(str, 6, -1)
```

*Help:* [expr-[:]](https://vimhelp.org/eval.txt.html#expr-[%3a]), [strpart()](https://vimhelp.org/builtin.txt.html#strpart%28%29), [strcharpart()](https://vimhelp.org/builtin.txt.html#strcharpart%28%29), [slice()](https://vimhelp.org/builtin.txt.html#slice%28%29)

### Counting the occurrences of a substring

**Python:**
```python
str = "Hello World"
c = str.count("l")
```

**Vim9Script:**
```vim
var str = "Hello World"
var c = str->count("l")
```

*Help:* [count()](https://vimhelp.org/builtin.txt.html#count%28%29)

### Finding the position of a substring

**Python:**
```python
str = "running"
idx = str.find("nn")    # leftmost
idx = str.rfind("ing")  # rightmost
# idx == -1 if the substring is not present
```

**Vim9Script:**
```vim
var str = "running"
var idx = str->stridx("nn")     # leftmost
idx = str->strridx("ing")       # rightmost
# idx == -1 if the substring is not present
```

*Help:* [stridx()](https://vimhelp.org/builtin.txt.html#stridx%28%29), [strridx()](https://vimhelp.org/builtin.txt.html#strridx%28%29)

### Checking whether a string starts with or ends with a substring

**Python:**
```python
str = "running"
if str.startswith("run"):
    print("starts with run")
if str.endswith("ing"):
    print("ends with ing")
```

**Vim9Script:**
```vim
var str = "running"
if str =~# '^run'
  echo "starts with run"
endif
if str[ : len('run') - 1] == 'run'
  echo "starts with run"
endif

if str =~ 'ing$'
  echo "ends with ing"
endif
```

*Help:* [expr-=~#](https://vimhelp.org/eval.txt.html#expr-=~#)

### Joining strings in a List with a separator

**Python:**
```python
s = ":".join(['ab', 'cd', 'ef'])
```

**Vim9Script:**
```vim
var s = join(['ab', 'cd', 'ef'], ':')
```

*Help:* [join()](https://vimhelp.org/builtin.txt.html#join%28%29)

### Changing the case of letters in a string

**Python:**
```python
s = "Hello World"
l = s.lower()
l = s.upper()
```

**Vim9Script:**
```vim
var s = "Hello World"
var l = s->tolower()
l = s->toupper()
```

*Help:* [tolower()](https://vimhelp.org/builtin.txt.html#tolower%28%29), [toupper()](https://vimhelp.org/builtin.txt.html#toupper%28%29)

### Replace a substring

**Python:**
```python
s = "Hello World"
s2 = s.replace("Hello", "New")
```

**Vim9Script:**
```vim
var s = "Hello World"
var s2 = s->substitute("Hello", "New", 'g')
```

*Help:* [substitute()](https://vimhelp.org/builtin.txt.html#substitute%28%29)

### Split a string

**Python:**
```python
s = "a:b:c"
s2 = s.split(":")
```

**Vim9Script:**
```vim
var s = "a:b:c"
var s2 = s->split(":")
```

*Help:* [split()](https://vimhelp.org/builtin.txt.html#split%28%29), [join()](https://vimhelp.org/builtin.txt.html#join%28%29),

### Stripping leading and trailing whitespace from a string

**Python:**
```python
s = "  python  "
# strip leading and trailing whitespace
s2 = s.strip()
print(f"<{s2}>")
# strip leading space characters
s2 = s.lstrip(' ')
print(f"<{s2}>")
# strip trailing space characters
s2 = s.rstrip(' ')
print(f"<{s2}>")
```

**Vim9Script:**
```vim
var s: string
s = "  vim  "
# strip leading and trailing whitespace
s2 = s->trim()
echo $"<{s2}>"
# strip leading space characters
s2 = s->trim(' ', 1)
echo $"<{s2}>"
# strip trailing space characters
s2 = s->trim(' ', 2)
echo $"<{s2}>"
```

*Help:* [trim()](https://vimhelp.org/builtin.txt.html#trim%28%29)

### Checking whether a string contains specific type of characters

**Python:**
```python
s = "text"
if s.isalnum():
    print("Contains only alphanumeric characters")
if s.isalpha():
    print("Contains only alphabetic characters")
if s.isdigit():
    print("Contains only digits")
if s.isspace():
    print("Contains only whitespace characters")
if s.isupper():
    print("Contains only uppercase characters")
if s.islower():
    print("Contains only lowercase characters")
```

**Vim9Script:**
```vim
var s = "text"
if s =~ '^[:alnum:]\+'
  echo "Contains only alphanumeric characters"
endif
if s =~ '^\a\+$'
  echo "Contains only alphabetic characters"
endif
if s =~ '^\d\+$'
  echo "Contains only digits"
endif
if s =~ '^\s\+$'
  echo "Contains only whitespace characters"
endif
if s =~ '^\u\+$'
  echo "Contains only uppercase characters"
endif
if s =~ '^\l\+$'
  echo "Contains only lowercase characters"
endif
```

*Help:* [/collection](https://vimhelp.org/pattern.txt.html#%2fcollection)

### Data type conversion to string

**Python:**
```python
s = str(268)
s = str(22.7)
s = str([1, 2, 3])
s = str({'a' : 1, 'b' : 2})
```

**Vim9Script:**
```vim
var s = string(268)
s = string(22.7)
s = string([1, 2, 3])
s = string({a: 1, b: 2})
```

*Help:* [string()](https://vimhelp.org/builtin.txt.html#string%28%29)

### Evaluating a string

**Python:**
```python
x = 10
y = eval("x * 2")
print(y)
n = eval("min([4, 3, 5])")
```

**Vim9Script:**
```vim
var x = 10
var y = eval("x * 2")
echo y
var n = eval("min([4, 3, 5])")
```

*Help:* [eval()](https://vimhelp.org/builtin.txt.html#eval%28%29)

### Executing commands in a string

**Python:**
```python
exec("for i in range(5):\n    print(i)\n")
```

**Vim9Script:**
```vim
execute "for i in range(5)\necho i\nendfor"
```

*Help:* [:execute](https://vimhelp.org/eval.txt.html#:execute)

### Substituting variables in a string (String Interpolation)

**Python:**
```python
aVar = 10
str = f"Value of 'a' is {aVar}"
print(str)

bList = [1, 2, 3]
str = f"bList is {bList}"
print(str)
```

**Vim9Script:**
```vim
var aVar = 10
var str = $"Value of 'a' is {aVar}"
echo str

var bList = [1, 2, 3]
str = $"bList is {string(bList)}"
echo str
```

*Help:* [interp-string](https://vimhelp.org/eval.txt.html#interp-string)

### Getting the Unicode value of a character and vice versa

**Python:**
```python
print("Ordinal value of 'a' is " + str(ord("a")))
print("Character with value 65 is " + chr(65))
```

**Vim9Script:**
```vim
echo "Ordinal value of 'a' is " .. char2nr('a')
echo "Character with value 65 is " .. nr2char(65)
```

*Help:* [char2nr()](https://vimhelp.org/builtin.txt.html#chr%28%29), [nr2char()](https://vimhelp.org/builtin.txt.html#or%28%29)

### Getting the character values of a string

**Python:**
```python
l = [ord(i) for i in 'Hello']
s = ''.join(chr(i) for i in l)
print(s)
print(l)
```

**Vim9Script:**
```vim
var l = str2list('Hello')
var s = list2str(l)
echo s
echo l
```

*Help:* [str2list()](https://vimhelp.org/builtin.txt.html#str2list%28%29), [list2str()](https://vimhelp.org/builtin.txt.html#list2str%28%29)

### Fuzzy matching strings

**Python:**
```python
from fuzzywuzzy import process
from fuzzywuzzy import fuzz

str_list = ['crow', 'clay', 'cobb']
m = process.extractOne('cay', str_list, scorer=fuzz.partial_ratio)
print(m)
```

**Vim9Script:**
```vim
var str_list = ['crow', 'clay', 'cobb']
var m = matchfuzzy(str_list, 'cay')
echo m
```

*Help:* [matchfuzzy()](https://vimhelp.org/builtin.txt.html#matchfuzzy%28%29), [matchfuzzypos()](https://vimhelp.org/builtin.txt.html#matchfuzzypos%28%29)

### String Methods

Method|Python|Vim9Script
----|------|---------
capitalize()| `'one two'.capitalize()` | `'one two'->substitute('.', '\u&', '')`
center()| `'abc'.center(10)` | `Not available`
count()| `"abbc".count('b')` | `"abbc"->count('b')`
decode()| `str.decode()` | `Not available`
encode()| `str.encode()` | `Not available`
endswith()| `'running'.endswith('ing')` | `'running' =~# 'ing$'`
expandtabs()| `"a\tb".expandtabs()` | `"a\tb"->substitute("\t", repeat(' ', 8), 'g')`
find()| `"running".find('nn')` | `"running"->stridx('nn')`
index()| `'hello'.index('e')` | `'hello'->stridx('e')`
isalnum()| `str.isalnum()` | `str =~ '^[[:alnum:]]\+'`
isalpha()| `str.isalpha()` | `str =~ '^\a\+$'`
isdigit()| `str.isdigit()` | `str =~ '^\d\+$'`
islower()| `str.islower()` | `str =~ '^\l\+$'`
isspace()| `str.isspace()` | `str =~ '^\s\+$'`
istitle()| `str.istitle()` | `str =~ '\(\<\u\l\+\>\)\s\?'`
isupper()| `str.isupper()` | `str =~ '^\u\+$'`
join()| `':'.join(['a', 'b', 'c'])` | `join(['a', 'b', 'c'], ':')`
ljust()| `'abc'.ljust(10)` | `Not available`
lower()| `'Hello'.lower()` | `'Hello'->tolower()`
lstrip()| `'  vim  '.lstrip()` | `'  vim  '->trim(' ', 1)`
partition()| `'ab-cd-ef'.partition('-')` | `'ab-cd-ef'->matchlist('\(.\{-}\)\(-\)\(.*\)')[1:3]`
replace()| `'abc'.replace('abc', 'xyz')` | `'abc'->substitute('abc', 'xyz', 'g')`
rfind()| `'running'.rfind('ing')` | `'running'->strridx('ing')`
rindex()| `'running'.rindex('ing')` | `'running'->strridx('ing')`
rjust()| `'abc'.rjust(10)` | `Not available`
rpartition()| `'ab-cd-ef'.rpartition('-')` | `'ab-cd-ef'->matchlist('\(.*\)\(-\)\(.*\)')[1:3]`
rsplit()| `'a-b-c-d'.rsplit('-', 2)` | `Not available`
rstrip()| `'  vim  '.rstrip()` | `'  vim  '->trim(' ', 2)`
split()| `'a-b-c'.split('-')` | `'a-b-c'->split('-')`
splitlines()| `"one\ntwo".splitlines()` | `"one\ntwo"->split("\n")`
startswith()| `'running'.startswith('run')` | `'running' =~# '^run'`
strip()| `'  vim  '.strip()` | `'  vim  '->trim()`
swapcase()| `'Abc'.swapcase()` | `Not available`
title()| `'onE twO'.title()` | `'onE twO'->substitute('\<\(.\)\(\S\+\)\>', '\u\1\L\2', 'g')`
translate()| `'abcd'.translate(string.maketrans('bd', '12'))` | `'abcd'->tr('bd', '12')`
upper()| `'Hello'.upper()` | `'Hello'->toupper()`
zfill()| `str.zfill(10)` | `printf("%010s", "Hello")`

*Help:* [string-functions](https://vimhelp.org/usr_41.txt.html#string-functions)

------------------------------------------------------------------------------

## List

### Creating a List

**Python:**
```python
l = [1, 2, 3, 4]
```

**Vim9script:**
```vim
var l = [1, 2, 3, 4]
```

*Help:* [List](https://vimhelp.org/eval.txt.html#List)

### Accessing a List element

**Python:**
```python
l = [1, 2, 3, 4]
v1 = l[2]
v2 = l[-2]
```

**Vim9Script:**
```vim
var l = [1, 2, 3, 4]
var v1 = l[2]
var v2 = l[-2]
```

*Help:* [list-index](https://vimhelp.org/eval.txt.html#list-index)

### Changing the value of a List element

**Python:**
```python
l = [1, 2, 3, 4]
l[3] = 5
```

**Vim9Script:**
```vim
var l = [1, 2, 3, 4]
var l[3] = 5
```

*Help:* [list-modification](https://vimhelp.org/eval.txt.html#list-modification)

### Adding an item to a List

**Python:**
```python
l = []
l.append(5)
l += [6, 7]
```

**Vim9Script:**
```vim
var l = []
add(l, 5)
var l += [5, 6]
```

*Help:* [add()](https://vimhelp.org/builtin.txt.html#add%28%29)

### Extending a List using another List

**Python:**
```python
l = []
l.extend([2, 3])
l += [6, 7]
```

**Vim9Script:**
```vim
var l = []
l->extend([2, 3])
l += [6, 7]
```

*Help:* [extend()](https://vimhelp.org/builtin.txt.html#extend%28%29)

### Inserting an item in a List

**Python:**
```python
l = [1, 3]
l.insert(1, 2)
```

**Vim9Script:**
```vim
var l = [1, 3]
# insert before index 1
eval l->insert(2, 1)
# insert at the begining
eval l->insert(5)
```

*Help:* [insert()](https://vimhelp.org/builtin.txt.html#insert%28%29)

### Removing an item from a List

**Python:**
```python
l = [4, 5, 6]
l.remove(5)
del l[0]
```

**Vim9Script:**
```vim
var l = [4, 5, 6]
var idx = index(l, 5)
if idx != -1
  remove(l, idx)
endif
unlet l[0]
echo l
```

*Help:* [remove()](https://vimhelp.org/builtin.txt.html#remove%28%29), [:unlet](https://vimhelp.org/eval.txt.html#%3aunlet)

### Removing the last element from a List

**Python:**
```python
l = [1, 2, 3]
v = l.pop()
```

**Vim9Script:**
```vim
var l = [1, 2, 3]
var v = l->remove(-1)
```

*Help:* [remove()](https://vimhelp.org/builtin.txt.html#remove%28%29)

### Find the index of an item in a List

**Python:**
```python
l = [1, 2, 3]
x = l.index(2)
```

**Vim9Script:**
```vim
var l = [1, 2, 3]
var x = l->index(2)
```

*Help:* [index()](https://vimhelp.org/builtin.txt.html#index%28%29)

### Find the index of an item in a List of Dicts by a Dict item value

**Python:**
```python
colors = [{'color': 'red'}, {'color': 'blue'}, {'color': 'green'}]
idx = next((i for i, v in enumerate(colors) if v['color'] == 'blue'), -1)
print(idx)
```

**Vim9Script:**
```vim
var colors = [{color: 'red'}, {color: 'blue'}, {color: 'green'}]
var idx = indexof(colors, (i, v) => v.color == 'blue')
echo idx
```

*Help:* [indexof()](https://vimhelp.org/builtin.txt.html#indexof%28%29)

### List slices

In Python, when using an index range to specify a series of items in a List, the item at the end index is not included. In Vim, the item at the end index is included.  The slice() function excludes the item at the end index.

**Python:**
```python
l = [1, 2, 3, 4]
print(l[1:3])
print(l[2:])
```

**Vim9Script:**
```vim
var l = [1, 2, 3, 4]
echo l[1 : 3]
echo l[2 : ]
echo l[-2 : ]

# slice() function excludes the item at the end index.
echo slice(l, 2, 3)
echo slice(l, 2)
```

*Help:* [sublist](https://vimhelp.org/eval.txt.html#sublist), [slice()](https://vimhelp.org/builtin.txt.html#slice%28%29)

### List Concatenation

**Python:**
```python
l = [1, 2] + [3 ,4]
```

**Vim9Script:**
```vim
var l = [1, 2] + [3, 4]
```

*Help:* [list-index](https://vimhelp.org/eval.txt.html#list-index)

### Adding multiple items to a List using repetition

**Python:**
```python
l = ['vim'] * 4
```

**Vim9Script:**
```vim
var l = ['vim']->repeat(4)
```

*Help:* [repeat()](https://vimhelp.org/builtin.txt.html#repeat%28%29)

### Count the number of occurrences of an item in a List

**Python:**
```python
l = [2, 4, 4, 5]
x = l.count(4)
```

**Vim9Script:**
```vim
var l = [2, 4, 4, 5]
var x = l->count(2)
```

*Help:* [count()](https://vimhelp.org/builtin.txt.html#count%28%29)

### Finding the List length

**Python:**
```python
l = ['a', 'b', 'c']
n = len(l)
```

**Vim9Script:**
```vim
var l = ['a', 'b', 'c']
var n = l->len()
```

*Help:* [len()](https://vimhelp.org/builtin.txt.html#len%28%29)

### List Iteration

**Python:**
```python
l = [10, 20, 30]
for v in l:
  print(v)

# Print both the list index and the value
for i, v in enumerate(l):
  print(i, v)

# Use a for loop for list iteration
for i in range(len(l)):
  print(i, l[i])
```

**Vim9Script:**
```vim
var l = [10, 20, 30]
for v in l
  echo v
endfor

# Print both the list index and the value
for [i, v2] in items(l)
  echo i v2
endfor

# Use a for loop for list iteration
for i2 in range(len(l))
  echo i l[i2]
endfor
```

*Help:* [:for](https://vimhelp.org/eval.txt.html#%3Afor)

### Sort a List

**Python:**
```python
l = [3, 2, 1]
l.sort()
print(l)
```

**Vim9Script:**
```vim
var l = [3, 2, 1]
echo l->sort()
```

*Help:* [sort()](https://vimhelp.org/builtin.txt.html#sort%28%29)

### Getting unique elements in a List

**Python:**
```python
l = ['a', 'b', 'b', 'c', 'c', 'd']
# order of the elements is not retained
tset = set(l)
print(list(tset))
```

**Vim9Script:**
```vim
# needs a sorted list
var l = ['a', 'b', 'b', 'c', 'c', 'd']
echo copy(l)->uniq()
```

*Help:* [uniq()](https://vimhelp.org/builtin.txt.html#uniq%28%29)

### Reverse a List

**Python:**
```python
l = [1, 2, 3]
l.reverse()
print(l)
```

**Vim9Script:**
```vim
var l = [1, 2, 3]
echo reverse(l)
```

*Help:* [reverse()](https://vimhelp.org/builtin.txt.html#reverse%28%29)

### Copying a List

**Python:**
```python
l = [3, 2, 1]
l2 = l.copy()
```

**Vim9Script:**
```vim
var l = [3, 2, 1]
var l2 = l->copy()
```

*Help:* [copy()](https://vimhelp.org/builtin.txt.html#copy%28%29)

### Deep Copying a List

**Python:**
```python
import copy
a = [[1, 2], [3, 4]]
b = copy.deepcopy(a)
```

**Vim9Script:**
```vim
var a = [[1, 2], [3, 4]]
var b = a->deepcopy()
```

*Help:* [deepcopy()](https://vimhelp.org/builtin.txt.html#deepcopy%28%29)

### Empty a List

**Python:**
```python
l = [3, 2, 1]
l.clear()
```

**Vim9Script:**
```vim
var l = [3, 2, 1]
unlet l[:]
```

*Help:* [:unlet](https://vimhelp.org/eval.txt.html#%3aunlet)

### Comparing two Lists

**Python:**
```python
l1 = [1, 2]
l2 = l1
l3 = [1, 2]
if l1 is l2:
  print("Lists l1 and l2 refer to the same list")
if l1 is not l3:
  print("Lists l1 and l3 do not refer to the same list")
if l1 == l3:
  print("Lists l1 and l3 contain the same elements")
if l1 != l3:
  print("Lists l1 and l3 are different")
```

**Vim9Script:**
```vim
var l1 = [1, 2]
var l2 = l1
var l3 = [1, 2]
if l1 is l2
  echo "Lists l1 and l2 refer to the same list"
endif
if l1 isnot l3
  echo "Lists l1 and l3 do not refer to the same list"
endif
if l1 == l3
  echo "Lists l1 and l3 contain the same elements"
endif
if l1 != l3
  echo "Lists l1 and l3 are different"
endif
```

*Help:* [list-identity](https://vimhelp.org/eval.txt.html#list-identity)

### Filter selected elements from a List

Note that Vim does not support List comprehension.

**Python:**
```python
odd = list(filter(lambda x: x % 2, range(10)))
odd = [x for x in range(10) if x % 2]
```

**Vim9Script:**
```vim
var odd = filter(range(10), (idx, v) => v % 2)
echo odd
```

*Help:* [filter()](https://vimhelp.org/builtin.txt.html#filter%28%29)

### Map List elements

Note that Vim does not support List comprehension.

**Python:**
```python
num_str = list(map(lambda x: str(x), range(10)))
num_str = [str(x) for x in range(10)]
```

**Vim9Script:**
```vim
var num_str = map(range(10), (idx, v) => string(v))
echo num_str
```

*Help:* [map()](https://vimhelp.org/builtin.txt.html#map%28%29)

### Reducing List elements

**Python:**
```python
# using a lambda function
from functools import reduce
sum = reduce((lambda x, y: x + y), [1, 2, 3, 4])

# using a function
def SumNum(a, b):
    return a + b
sum = reduce(SumNum, [1, 2, 3, 4])
```

**Vim9Script:**
```vim
# using a lambda function
var sum = reduce([1, 2, 3, 4], (x, y) => x + y)

# using a function
def SumNum(x: number, y: number): number
  return x + y
enddef
var sum2 = reduce([1, 2, 3, 4], function('SumNum'))
echo sum2
```

*Help:* [reduce()](https://vimhelp.org/builtin.txt.html#reduce%28%29)

### Flattening a List

**Python:**
```python
l = [[5, 9], [1, 3], [10, 20]]
flattenlist = [i for subl in l for i in subl]
print(flattenlist)
```

**Vim9Script:**
```vim
var l = [[5, 9], [1, 3], [10, 20]]
var flattenlist = flattennew(l)
echo flattenlist
```

*Help:* [flatten()](https://vimhelp.org/builtin.txt.html#flatten%28%29), [flattennew()](https://vimhelp.org/builtin.txt.html#flattennew%28%29)

### Finding the mix/max value in a List

**Python:**
```python
l = [3, 10, 8]
v1 = min(l)
v2 = max(l)
print(v1, v2)
```

**Vim9Script:**
```vim
var l = [3, 10, 8]
var v1 = l->min()
var v2 = l->max()
echo v1 v2
```

*Help:* [max()](https://vimhelp.org/builtin.txt.html#max%28%29), [min()](https://vimhelp.org/builtin.txt.html#min%28%29)

### Converting a List to a string

**Python:**
```python
s = str([3, 5, 7])
```

**Vim9Script:**
```vim
var s = string([3, 5, 7])
echo s
```

*Help:* [string()](https://vimhelp.org/builtin.txt.html#string%28%29)

### List Methods

Method|Python|Vim9Script
----|------|---------
append()| `m.append(6)` | `m->add(6)`
clear()| `m.clear()` | `unlet m[:]`
copy()| `m.copy()` | `m->copy()`
count()| `m.count(6)` | `m->count(6)`
extend()| `m.extend([5, 6])` | `m->extend([5, 6])`
index()| `m.index(6)` | `m-index(6)`
insert()| `m.insert(2, 9)` | `m->insert(9, 2)`
pop()| `m.pop()` | `m->remove(-1)`
remove()| `m.remove(6)` | `m->remove(m->index(6))`
reverse()| `m.reverse()` | `m->reverse()`
sort()| `m.sort()` | `m->sort()`

*Help:* [list-functions](https://vimhelp.org/usr_41.txt.html#list-functions)

------------------------------------------------------------------------------

## Tuple

### Creating a Tuple

**Python:**
```python
t = (1, 2, 3, 4)
```

**Vim9Script:**
```vim
var t = (1, 2, 3, 4)
```

*Help:* [Tuple](https://vimhelp.org/eval.txt.html#Tuple)

### Accessing a Tuple element

**Python:**
```python
t = (1, 2, 3, 4)
v1 = t[2]
v2 = t[-2]
```

**Vim9Script:**
```vim
var t = (1, 2, 3, 4)
var v1 = t[2]
var v2 = t[-2]
```

*Help:* [tuple-index](https://vimhelp.org/eval.txt.html#tuple-index)

### Changing the value of a List/Dict item in a Tuple

**Python:**
```python
t = (1, [2, 3], {'a': 4})
t[1][0] = 5
t[2]['a'] = 10
```

**Vim9Script:**
```vim
var t = (1, [2, 3], {'a': 6})
t[1][0] = 5
t[2]['a'] = 10
```

*Help:* [tuple-modification](https://vimhelp.org/eval.txt.html#tuple-modification)

### Find the index of an item in a Tuple

**Python:**
```python
t = (1, 2, 3)
x = t.index(2)
```

**Vim9Script:**
```vim
var t = (1, 2, 3)
var x = t->index(2)
echo x
```

*Help:* [index()](https://vimhelp.org/builtin.txt.html#index%28%29)

### Find the index of an item in a Tuple of Dicts by a Dict item value

**Python:**
```python
colors = ({'color': 'red'}, {'color': 'blue'}, {'color': 'green'})
idx = next((i for i, v in enumerate(colors) if v['color'] == 'blue'), -1)
print(idx)
```

**Vim9Script:**
```vim
var colors = ({color: 'red'}, {color: 'blue'}, {color: 'green'})
var idx = indexof(colors, (i, v) => v.color == 'blue')
echo idx
```

*Help:* [indexof()](https://vimhelp.org/builtin.txt.html#indexof%28%29)

### Tuple slices

In Python, when using an index range to specify a series of items in a Tuple, the item at the end index is not included. In Vim, the item at the end index is included.  The slice() function excludes the item at the end index.

**Python:**
```python
t = (1, 2, 3, 4)
print(t[1:3])
print(t[2:])
```

**Vim9Script:**
```vim
var t = (1, 2, 3, 4)
echo t[1 : 3]
echo t[2 :]
echo t[-2 :]

# slice() function excludes the item at the end index.
echo slice(t, 2, 3)
echo slice(t, 2)
```

*Help:* [subtuple](https://vimhelp.org/eval.txt.html#subtuple), [slice()](https://vimhelp.org/builtin.txt.html#slice%28%29)

### Tuple Concatenation

**Python:**
```python
t = (1, 2) + (3 ,4)
```

**Vim9Script:**
```vim
var t = (1, 2) + (3, 4)
echo t
```

*Help:* [tuple-concatenation](https://vimhelp.org/eval.txt.html#tuple-concatenation)

### Adding multiple items to a Tuple using repetition

**Python:**
```python
t = ('vim',) * 4
```

**Vim9Script:**
```vim
var t = ('vim')->repeat(4)
echo t
```

*Help:* [repeat()](https://vimhelp.org/builtin.txt.html#repeat%28%29)

### Count the number of occurrences of an item in a Tuple

**Python:**
```python
t = (2, 4, 4, 5)
x = t.count(4)
```

**Vim9Script:**
```vim
var t = (2, 4, 4, 5)
var x = t->count(2)
```

*Help:* [count()](https://vimhelp.org/builtin.txt.html#count%28%29)

### Finding the Tuple length

**Python:**
```python
t = ('a', 'b', 'c')
n = len(t)
```

**Vim9Script:**
```vim
var t = ('a', 'b', 'c')
var n = t->len()
```

*Help:* [len()](https://vimhelp.org/builtin.txt.html#len%28%29)

### Tuple Iteration

**Python:**
```python
t = (10, 20, 30)
for v in t:
  print(v)

# Print both the tuple index and the value
for i, v in enumerate(t):
  print(i, v)

# Use a for loop for tuple iteration
for i in range(len(t)):
  print(i, t[i])
```

**Vim9Script:**
```vim
var t = (10, 20, 30)
for v in t
  echo v
endfor

# Print both the tuple index and the value
for [i, v2] in items(t)
  echo i v2
endfor

# Use a for loop for tuple iteration
for i2 in range(len(t))
  echo i2 t[i2]
endfor
```

*Help:* [:for](https://vimhelp.org/eval.txt.html#%3Afor)

### Reverse a Tuple

**Python:**
```python
t = (1, 2, 3)
t.reverse()
print(t)
```

**Vim9Script:**
```vim
var t = (1, 2, 3)
echo reverse(t)
```

*Help:* [reverse()](https://vimhelp.org/builtin.txt.html#reverse%28%29)

### Copying a Tuple

**Python:**
```python
t = (3, 2, 1)
t2 = t.copy()
```

**Vim9Script:**
```vim
var t = (3, 2, 1)
var t2 = t->copy()
```

*Help:* [copy()](https://vimhelp.org/builtin.txt.html#copy%28%29)

### Deep Copying a Tuple

**Python:**
```python
import copy
a = ([1, 2], [3, 4])
b = copy.deepcopy(a)
```

**Vim9Script:**
```vim
var a = ([1, 2], [3, 4])
var b = a->deepcopy()
```

*Help:* [deepcopy()](https://vimhelp.org/builtin.txt.html#deepcopy%28%29)

### Comparing two Tuples

**Python:**
```python
t1 = (1, 2)
t2 = l1
t3 = (1, 2)
if t1 is t2:
  print("Tuples t1 and t2 refer to the same tuple")
if t1 is not t3:
  print("Tuples t1 and t3 do not refer to the same tuple")
if t1 == t3:
  print("Tuples t1 and t3 contain the same elements")
if t1 != t3:
  print("Tuples t1 and t3 are different")
```

**Vim9Script:**
```vim
var t1 = (1, 2)
var t2 = t1
var t3 = (1, 2)
if t1 is t2
  echo "Tuples t1 and t2 refer to the same tuple"
endif
if t1 isnot t3
  echo "Tuples t1 and t3 do not refer to the same tuple"
endif
if t1 == t3
  echo "Tuples t1 and t3 contain the same elements"
endif
if t1 != t3
  echo "Tuples t1 and t3 are different"
endif
```

*Help:* [tuple-identity](https://vimhelp.org/eval.txt.html#tuple-identity)

### Reducing Tuple elements

**Python:**
```python
# using a lambda function
from functools import reduce
sum = reduce((lambda x, y: x + y), (1, 2, 3, 4))

# using a function
def SumNum(a, b):
    return a + b
sum = reduce(SumNum, (1, 2, 3, 4))
```

**Vim9Script:**
```vim
# using a lambda function
var sum = reduce((1, 2, 3, 4), (x, y) => x + y)

# using a function
def SumNum(x: number, y: number): number
  return x + y
enddef
var sum2 = reduce((1, 2, 3, 4), function('SumNum'))
```

*Help:* [reduce()](https://vimhelp.org/builtin.txt.html#reduce%28%29)

### Finding the mix/max value in a Tuple

**Python:**
```python
t = (3, 10, 8)
v1 = min(t)
v2 = max(t)
print(v1, v2)
```

**Vim9Script:**
```vim
var t = (3, 10, 8)
var v1 = t->min()
var v2 = t->max()
echo v1 v2
```

*Help:* [max()](https://vimhelp.org/builtin.txt.html#max%28%29), [min()](https://vimhelp.org/builtin.txt.html#min%28%29)

### Converting a Tuple to a string

**Python:**
```python
s = str((3, 5, 7))
```

**Vim9Script:**
```vim
var s = string((3, 5, 7))
```

*Help:* [string()](https://vimhelp.org/builtin.txt.html#string%28%29)

### Tuple Methods

Method|Python|Vim9Script
----|------|---------
copy()| `m.copy()` | `m->copy()`
count()| `m.count(6)` | `m->count(6)`
index()| `m.index(6)` | `m-index(6)`
reverse()| `m.reverse()` | `m->reverse()`

*Help:* [tuple-functions](https://vimhelp.org/usr_41.txt.html#tuple-functions)

------------------------------------------------------------------------------

## Dictionary

### Creating a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
x = {}
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
var x = {}
```

*Help:* [Dict](https://vimhelp.org/eval.txt.html#Dict)

### Retrieving the value of a Dict item

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
x = d['red']
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
var x = d['red']
x = d.red
```

*Help:* [dict](https://vimhelp.org/eval.txt.html#dict)

### Changing the value of a Dict item

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
d['red'] = 15
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
d.red = 15
```

*Help:* [dict-modification](https://vimhelp.org/eval.txt.html#dict-modification)

### Accessing a Dict item

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
v = d.get('red')
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
var v = d->get('red')
```

*Help:* [get()](https://vimhelp.org/builtin.txt.html#get%28%29)

### Adding a new Dict item

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
d['green'] = 30
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
d.green = 30
```

*Help:* [dict-modification](https://vimhelp.org/eval.txt.html#dict-modification)

### Extending a Dict using another Dict

**Python:**
```python
d = {}
d.update({'color' : 'grey'})
```

**Vim9Script:**
```vim
var d = {}
eval d->extend({color: 'grey'})
```

*Help:* [extend()](https://vimhelp.org/builtin.txt.html#extend%28%29)

### Removing an item from a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
d.pop('red')
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
d->remove('red')
```

*Help:* [remove()](https://vimhelp.org/builtin.txt.html#remove%28%29),

### Clearing all the items from a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
d.clear()
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
d->filter("0")
```

*Help:* [dict-modification](https://vimhelp.org/eval.txt.html#dict-modification)

### Getting the size of a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
n = len(d)
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
var n = d->len()
```

*Help:* [len()](https://vimhelp.org/builtin.txt.html#len%28%29)

### Count the number of occurrences of a value in a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 10}
x = sum(n == 10 for n in d.values())
print(x)
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 10}
var x = d->count(10)
echo x
```

*Help:* [count()](https://vimhelp.org/builtin.txt.html#count%28%29)

### Checking Dict membership

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
if 'red' in d:
    print("found red key")
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
if d->has_key('red')
  echo "found red key"
endif
```

*Help:* [has_key()](https://vimhelp.org/builtin.txt.html#has_key%28%29)

### Iterating through all the keys in a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
for k in d:
    print(k)
for k in d:
    print(d[k])
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
for k in d->keys()
  echo k
endfor
for k2 in d->keys()
  echo d[k2]
endfor
```

*Help:* [keys()](https://vimhelp.org/builtin.txt.html#keys%28%29)

### Iterating through all the values in a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
for v in d.values():
    print(v)
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
for v in d->values()
  echo v
endfor
```

*Help:* [values()](https://vimhelp.org/builtin.txt.html#values%28%29)

### Iterating through all the key, value pairs in a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
for k, v in d.items():
    print(k, v)
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
for [k, v] in d->items()
  echo k v
endfor
```

*Help:* [items()](https://vimhelp.org/builtin.txt.html#items%28%29)

### Copying a Dict

**Python:**
```python
d = {'red' : 10, 'blue' : 20}
new_d = d.copy()
```

**Vim9Script:**
```vim
var d = {red: 10, blue: 20}
var new_d = d->copy()
```

*Help:* [copy()](https://vimhelp.org/builtin.txt.html#copy%28%29)

### Deep Copying a Dict

**Python:**
```python
import copy
a = {'x' : [1, 2], 'y' : [3, 4]}
b = copy.deepcopy(a)
```

**Vim9Script:**
```vim
var a = {x: [1, 2], y: [3, 4]}
var b = a->deepcopy()
```

*Help:* [deepcopy()](https://vimhelp.org/builtin.txt.html#deepcopy%28%29)

### Comparing two Dicts

**Python:**
```python
d1 = {'a' : 10, 'b' : 20}
d2 = {'a' : 10, 'b' : 20}
if d1 == d2:
    print("Dicts d1 and d2 have the same content")
d3 = d1
if d1 is d3:
  print("Dicts d1 and d3 refer to the same dict")
if d2 is not d3:
  print("Dicts d2 and d3 do not refer to the same dict")
```

**Vim9Script:**
```vim
var d1 = {a: 10, b: 20}
var d2 = {a: 10, b: 20}
if d1 == d2
  echo "Dicts d1 and d2 have the same content"
endif
var d3 = d1
if d1 is d3
  echo "Dicts d1 and d3 refer to the same dict"
endif
if d2 isnot d3
  echo "Dicts d2 and d3 do not refer to the same dict"
endif
```

*Help:* [dict-identity](https://vimhelp.org/eval.txt.html#dict-identity)

### Filter selected elements from a Dict

Note that Vim does not support Dict comprehension.

**Python:**
```python
d1 = {'red' : 10, 'green' : 20, 'blue' : 10}
# filter dict items with value 10
d2 = dict(filter(lambda e : e[1] == 10, d1.items()))
print(d1, d2)
# use dict comprehension
d3 = {k: v for [k, v] in d1.items() if v == 10}
print(d1, d3)
```

**Vim9Script:**
```vim
var d1 = {red: 10, green: 20, blue: 10}
# filter dict items with value 10
var d2 = copy(d1)->filter((k, v) => v == 10)
echo d1 d2
```

*Help:* [filter()](https://vimhelp.org/builtin.txt.html#filter%28%29)

### Map Dict elements

Note that Vim does not support Dict comprehension.

**Python:**
```python
d1 = {'red' : 10, 'green' : 20, 'blue' : 30}
# increment the values by 5
d2 = dict(map(lambda e : (e[0], e[1] + 5), d1.items()))
print(d1, d2)
# use dict comprehension
d3 = {k: v + 5 for k, v in d1.items()}
print(d1, d3)
```

**Vim9Script:**
```vim
var d1 = {red: 10, green: 20, blue: 30}
# increment the values by 5
var d2 = copy(d1)->map((k, v) => v + 5)
echo d1 d2
```

*Help:* [map()](https://vimhelp.org/builtin.txt.html#map%28%29)

### Finding the min/max value in a Dict

**Python:**
```python
d = {'red' : 10, 'green' : 20}
v1 = min(d.values())
v2 = max(d.values())
print(v1, v2)
```

**Vim9Script:**
```vim
var d = {red: 10, green: 20}
var v1 = d->min()
var v2 = d->max()
echo v1 v2
```

*Help:* [max()](https://vimhelp.org/builtin.txt.html#max%28%29), [min()](https://vimhelp.org/builtin.txt.html#min%28%29)

### Converting a Dict to a string

**Python:**
```python
d = str({'a' : 1, 'b' : 2})
```

**Vim9Script:**
```vim
var d = string({a: 1, b: 2})
```

*Help:* [string()](https://vimhelp.org/builtin.txt.html#string%28%29)

### Dictionary Methods

Method|Python|Vim9Script
----|------|---------
clear()| `d.clear()` | `filter(d, '0')`
copy()| `newDict = d.copy()` | `var newDict = d->copy()`
fromkeys()| `d = dict.fromkeys(x)` | `Not available`
get()| `v = d.get('red')` | `var v = d->get('red')`
in or has_key() | `'red' in d` | `d->has_key('red')`
items()| `d.items()` | `d->items()`
keys()| `d.keys()` | `d->keys()`
pop()| `d.pop('red')` | `d->remove('red')`
popitem()| `d.popitem()` | `Not available`
setdefault()| `d.setdefault('red', 10)` | `Not available`
update()| `d.update({'a' : 10, 'b' : 20}` | `d->extend({'a' : 10, 'b' : 20})`
values()| `d.values()` | `d->values()`

*Help:* [dict-functions](https://vimhelp.org/usr_41.txt.html#dict-functions)

------------------------------------------------------------------------------

## Enum

### Defining an Enum

**Python:**
```python
from enum import Enum

class Color(Enum):
  RED = 1
  GREEN = 2
  BLUE = 3
```

**Vim9Script:**
```vim
enum Color
  RED,
  GREEN,
  BLUE
endenum
```

*Help:* [enum](https://vimhelp.org/vim9class.txt.html#enum)

### Using a Enum value

**Python:**
```python
from enum import Enum

class Color(Enum):
  RED = 1
  GREEN = 2
  BLUE = 3

print(Color.RED)
```

**Vim9Script:**
```vim
enum Color
  RED,
  GREEN,
  BLUE
endenum

echo Color.RED
```

*Help:* [enumvalue](https://vimhelp.org/vim9class.txt.html#enumvalue)

### Accessing Enum name and ordinal

**Python:**
```python
from enum import Enum

class Color(Enum):
  RED = 1
  GREEN = 2
  BLUE = 3

print(Color.GREEN.name)
print(Color.GREEN.value)
```

**Vim9Script:**
```vim
enum Color
  RED,
  GREEN,
  BLUE
endenum

echo Color.GREEN.name
echo Color.GREEN.ordinal
```

*Help:* [enum-name](https://vimhelp.org/vim9class.txt.html#enum-name), [enum-ordinal](https://vimhelp.org/vim9class.txt.html#enum-ordinal)

### Iterating through enum values

**Python:**
```python
from enum import Enum

class Color(Enum):
  RED = 1
  GREEN = 2
  BLUE = 3

for e in Color:
  print(e)
```

**Vim9Script:**
```vim
enum Color
  RED,
  GREEN,
  BLUE
endenum

for e in Color.values
  echo e.name
endfor
```

*Help:* [enum-values](https://vimhelp.org/vim9class.txt.html#enum-values)

------------------------------------------------------------------------------

## If statement

### Basic If statement

**Python:**
```python
if a > b:
    print("a is greater than b")
```

**Vim9Script:**
```vim
if a > b
  echo "a is greater than b"
endif
```

*Help:* [:if](https://vimhelp.org/eval.txt.html#%3aif)

### If-else statement

**Python:**
```python
if a > b:
    print("a is greater than b")
else:
    print("a is less than or equal to b")
```

**Vim9Script:**
```vim
if a > b
  echo "a is greater than b"
else
  echo "a is less than or equal to b"
endif
```

*Help:* [:else](https://vimhelp.org/eval.txt.html#%3aelse)

### If-elseif statement

**Python:**
```python
if a > b:
    print("a is greater than b")
elif a < b:
    print("a is less than b")
else:
    print("a is equal to b")
```

**Vim9Script:**
```vim
if a > b
  echo "a is greater than b"
elseif a < b
  echo "a is less than b"
else
  echo "a is equal to b"
endif
```

*Help:* [:elseif](https://vimhelp.org/eval.txt.html#%3aelseif)

### Checking multiple conditions

**Python:**
```python
if a > b and (a > c or a > d):
    print "a is greater than b and greater than c or d"
```

**Vim9Script:**
```vim
if a > b && (a > c || a > d)
  echo "a is greater than b and greater than c or d"
endif
```

*Help:* [expr2](https://vimhelp.org/eval.txt.html#expr2)

### Nested If statements
**Python:**
```python
if status == True:
    if a >= 1:
        print("Nested if")
```

**Vim9Script:**
```vim
if status == true
  if a >= 1
    echo "Nested if"
  endif
endif
```

*Help:* [:if](https://vimhelp.org/eval.txt.html#%3aif)

------------------------------------------------------------------------------

## For Loop

**Python:**
```python
for i in range(5):
    print(i)
```

**Vim9Script:**
```vim
for i in range(5)
  echo i
endfor
```

*Help:* [:for](https://vimhelp.org/eval.txt.html#%3afor)

### Breaking out of a For loop
**Python:**
```python
for i in ['a', 'b', 'c']:
    if i == 'b':
        break
    print(i)
```

**Vim9Script:**
```vim
for i in ['a', 'b', 'c']
  if i == 'b'
    break
  endif
  echo i
endfor
```

*Help:* [:break](https://vimhelp.org/eval.txt.html#%3abreak)

### Continuing a For loop
**Python:**
```python
for i in ['a', 'b', 'c']:
    if i == 'b':
        continue
    print(i)
```

**Vim9Script:**
```vim
for i in ['a', 'b', 'c']
  if i == 'b'
    continue
  endif
  echo i
endfor
```

*Help:* [:continue](https://vimhelp.org/eval.txt.html#%3acontinue)

### Nested For Loops
**Python:**
```python
for i in range(10):
    for j in range(10):
        print(str(i) + 'x' + str(j) + '=' + str(i * j))
```

**Vim9Script:**
```vim
for i in range(4)
  for j in range(4)
    echo $"{i} x {j} = {i * j}"
  endfor
endfor
```

*Help:* [:for](https://vimhelp.org/eval.txt.html#%3afor)

------------------------------------------------------------------------------

## While Loop

**Python:**
```python
i = 1
while i <= 5 :
   print(i)
   i += 1
```

**Vim9Script:**
```vim
var i = 1
while i <= 5
  echo i
  i += 1
endwhile
```

*Help:* [:while](https://vimhelp.org/eval.txt.html#%3awhile)

------------------------------------------------------------------------------

## Comment

**Python:**
```python
# This is a python comment
i = 0    # First iteration
```

**Vim9Script:**
```vim
# This is a Vim9script comment
var i = 0    # First iteration
```

*Help:* [:comment](https://vimhelp.org/cmdline.txt.html#%3acomment)

------------------------------------------------------------------------------

## Function

Functions in a Vim9script are automatically compiled.  A Vim9 function name must start with an uppercase letter.

### Defining a Function

**Python:**
```python
def Min(x, y):
    return x if < y else y

print(Min(6, 3)
```

**Vim9Script:**
```vim
def Min(x: number, y: number): number
  return x < y ? x : y
enddef

echo Min(6, 3)
```

*Help:* [user-functions](https://vimhelp.org/eval.txt.html#user-functions)

### Calling a Function

**Python:**
```python
def EchoValue(v):
    print(v)

EchoValue(100)
```

**Vim9Script:**
```vim
def EchoValue(v: number)
  echo v
enddef

EchoValue(100)
```

*Help:* [:call](https://vimhelp.org/eval.txt.html#%3acall)

### Function return value

**Python:**
```python
def Sum(a, b):
    return a + b

s = Sum(10, 20)
```

**Vim9Script:**
```vim
def Sum(a: number, b: number): number
  return a + b
enddef

var s = Sum(10, 20)
```

*Help:* [:return](https://vimhelp.org/eval.txt.html#%3areturn)

### Pass by reference

**Python:**
```python
def AddValues(l):
    l.extend([1, 2, 3, 4])
```

**Vim9Script:**
```vim
def AddValues(l: list<number>)
  l->extend([1, 2, 3, 4])
enddef
```

*Help:* [function-argument](https://vimhelp.org/eval.txt.html#function-argument)

### Variable number of arguments

**Python:**
```python
def Sum(v1, *args):
    sum = v1
    for i in *args:
        sum += i
    return sum
```

**Vim9Script:**
```vim
def Sum(v1: number, ...rest: list<number>): number
  var sum = v1
  for i in rest
    sum += i
  endfor
  return sum
enddef
var s1 = Sum(10, 20, 30, 40)
s1 = Sum(10)
```

*Help:* [a:000](https://vimhelp.org/eval.txt.html#a%3a000)

### Default value for arguments

**Python:**
```python
def Powerof(base, exp = 2):
    return base ** exp
```

**Vim9Script:**
```vim
def PowerOf(base: number, exp: number = 2): number
  return float2nr(pow(base, exp))
enddef
var val = PowerOf(4)
echo val
val = PowerOf(4, 3)
echo val
```

*Help:* [optional-function-argument](https://vimhelp.org/eval.txt.html#optional-function-argument)

### Accessing global variables

**Python:**
```python
counter = 1
def Incr():
    global counter
    counter += 1
```

**Vim9Script:**
```vim
g:counter = 1
def Incr()
  g:counter += 1
enddef
Incr()
echo g:counter
```

*Help:* [global-variable](https://vimhelp.org/eval.txt.html#global-variable)

### Function reference

**Python:**
```python
def Foo():
    print("Foo")
Bar = Foo
Bar()
```

**Vim9Script:**
```vim
def Foo()
  echo "Foo"
enddef
var Bar = function("Foo")
Bar()
```

*Help:* [Funcref](https://vimhelp.org/eval.txt.html#Funcref)

------------------------------------------------------------------------------

## Lambda Function

**Python:**
```python
F = lambda x , y: x - y
print(F(5,2))
```

**Vim9Script:**
```vim
var F = (x, y) => x - y
echo F(5, 2)
```

*Help:* [lambda](https://vimhelp.org/eval.txt.html#lambda)

------------------------------------------------------------------------------

## Partial Function

**Python:**
```python
import functools
def Mylog(subsys, msg):
    print("%s: %s" % (subsys, msg))

ErrLog = functools.partial(Mylog, 'ERR')
ErrLog("Failed to open file")
```

**Vim9Script:**
```vim
def Mylog(subsys: string, msg: string)
  echo printf("%s: %s", subsys, msg)
enddef

var ErrLog = function('Mylog', ['ERR'])
ErrLog("Failed to open file")
```

*Help:* [Partial](https://vimhelp.org/eval.txt.html#Partial)

------------------------------------------------------------------------------

## Closure

### Closure with a lambda function

**Python:**
```python
def foo(arg):
  i = 3
  return lambda x: x + i - arg

bar = foo(4)
print(bar(6))
```

**Vim9Script:**
```vim
def Foo(arg: number): func
  var i = 3
  return (x) => x + i - arg
enddef

var Bar = Foo(4)
echo Bar(6)
```

*Help:* [closure](https://vimhelp.org/eval.txt.html#closure)

### Closure with a function reference

**Python:**
```python
def Foo(base):
    def Bar(val):
        return base + val
    return Bar

F = Foo(10)
print(F(2))
F = Foo(20)
print(F(2))
```

**Vim9Script:**
```vim
def Foo(base: number): func
  def g:Bar(val: number): number
    return base + val
  enddef
  return funcref('g:Bar')
enddef

var F = Foo(10)
echo F(2)
F = Foo(20)
echo F(2)
```

*Help:* [func-closure](https://vimhelp.org/eval.txt.html#%3Afunc-closure)

------------------------------------------------------------------------------

## Class

### Defining a class

**Python:**
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

**Vim9Script:**
```vim
class Point
  var x: number
  var y: number

  def new(x: number, y: number)
    this.x = x
    this.y = y
  enddef
endclass
```

*Help:* [vim9-class](https://vimhelp.org/vim9class.txt.html#vim9-class)

### Instantiating a class

**Python:**
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(10, 20)
```

**Vim9Script:**
```vim
class Point
  var x: number
  var y: number
endclass

var p = Point.new(10, 20)
```

*Help:* [vim9-class](https://vimhelp.org/vim9class.txt.html#new())

### Class Methods and Class Variables

**Python:**
```python
class Student():
    next_ID = 0

    @classmethod
    def getNextID(cls):
        cls.next_ID += 1
        return cls.next_ID

print(Student.getNextID())
```

**Vim9Script:**
```vim
class Student
  static var next_ID: number = 0

  static def GetNextID(): number
    next_ID += 1
    return next_ID
  enddef
endclass

echo Student.GetNextID()
```

*Help:* [class-method](https://vimhelp.org/vim9class.txt.html#class-method), [Vim9-class-member](https://vimhelp.org/vim9class.txt.html#Vim9-class-member)

### Extending a class

**Python:**
```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)
        self.breed = breed

    def Show(self):
        print(f"name = {self.name}, breed = {self.breed}")

a = Dog('Cooper', 'Beagle')
a.Show()
```

**Vim9Script:**
```vim
class Animal
    var name: string

    def new(name: string)
        this.name = name
    enddef
endclass

class Dog extends Animal
    var breed: string

    def new(name: string, breed: string)
	this.name = name
        this.breed = breed
    enddef

    def Show()
        echo $"name = {this.name}, breed = {this.breed}"
    enddef
endclass

var a = Dog.new('Cooper', 'Beagle')
a.Show()
```

*Help:* [extends](https://vimhelp.org/vim9class.txt.html#extends)

### Abstract class and methods

**Python:**
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass

class Square(Shape):
    def __init__(self, side):
        self.side = side

    def area(self):
        return self.side * self.side

    def perimeter(self):
        return 4 * self.side

square = Square(4)
print(f"Square area: {square.area()}")
print(f"Square perimeter: {square.perimeter()}")
```

**Vim9Script:**
```vim
abstract class Shape
  abstract def Area(): number
  abstract def Perimeter(): number
endclass

class Square extends Shape
  var side: number

  def new(side: number)
    this.side = side
  enddef

  def Area(): number
    return this.side * this.side
  enddef

  def Perimeter(): number
    return 4 * this.side
  enddef
endclass

var square = Square.new(4)
echo $"Square area: {square.Area()}"
echo $"Square perimeter: {square.Perimeter()}"
```

*Help:* [Vim9-abstract-class](https://vimhelp.org/vim9class.txt.html#Vim9-abstract-class), [abstract-method](https://vimhelp.org/vim9class.txt.html#abstract-method)

### Interface

**Python:**
```python
from abc import ABC, abstractmethod

class Intf1(ABC):
    @abstractmethod
    def Method1(self, param1):
        pass

class Intf2(ABC):
    @abstractmethod
    def Method2(self, param1):
        pass

class SomeClass(Intf1, Intf2):
    def Method1(self, param1):
        print(f"Method1: param = {param1}")

    def Method2(self, param1):
        print(f"Method2: param = {param1}")

c = SomeClass()
c.Method1('foo')
c.Method2('bar')
```

**Vim9Script:**
```vim
vim9script

interface Intf1
  def Method1(param1: string)
endinterface

interface Intf2
  def Method2(param1: string)
endinterface

class SomeClass implements Intf1, Intf2
  def Method1(param1: string)
    echo $"param = {param1}"
  enddef

  def Method2(param1: string)
    echo $"param = {param1}"
  enddef
endclass

var c = SomeClass.new()
c.Method1('foo')
c.Method2('bar')
```

*Help:* [Interface](https://vimhelp.org/vim9class.txt.html#Interface)

------------------------------------------------------------------------------

## Exception Handling

### Basic exception handling

**Python:**
```python
try:
    f = open('buf.java', 'r')
    lines = f.readlines()
    f.close()
except IOError:
    print("Failed to open file")
```

**Vim9Script:**
```vim
try
  var l = readfile('buf.java')
catch /E484:/
  echo "Failed to read file"
endtry
```

*Help:* [exception-handling](https://vimhelp.org/eval.txt.html#exception-handling)

### Catching all exceptions

**Python:**
```python
try:
    f = open('buf.java', 'r')
    lines = f.readlines()
    f.close()
except Exception as e:
    print("Caught " + str(e))
```

**Vim9Script:**
```vim
try
  var l = readfile('buf.java')
catch
  echo $"Caught {v:exception}"
endtry
```

*Help:* [catch-errors](https://vimhelp.org/eval.txt.html#catch-errors)

### Executing code after a try block (finally)

**Python:**
```python
try:
    f = open('buf.java', 'r')
    lines = f.readlines()
    f.close()
finally:
    print("executing code in finally block")
```

**Vim9Script:**
```vim
try
  var l = readfile('buf.java')
finally
  echo "executing code in finally block"
endtry
```

*Help:* [try-finally](https://vimhelp.org/eval.txt.html#try-finally)

### Raising a custom exception

**Python:**
```python
try:
    raise Exception('MyException')
except Exception as e:
    if str(e) == 'MyException':
        print("Caught MyException")
finally:
    print("Finally block")
```

**Vim9Script:**
```vim
try
  throw 'MyException'
catch /MyException/
  echo "Caught MyException"
finally
  echo "Finally block"
endtry
```

*Help:* [throw-catch](https://vimhelp.org/eval.txt.html#throw-catch)

------------------------------------------------------------------------------

## Line Continuation

**Python:**
```python
a = 1 + 2 + 3 + \
      4 + 5 + 6
print(a)
```

**Vim9Script:**
```vim
var a = 1 + 2 + 3 +
        4 + 5 + 6
echo a
```

*Help:* [line-continuation](https://vimhelp.org/eval.txt.html#line-continuation)

------------------------------------------------------------------------------

## File Operations

### Reading all the lines from a file

Vim has a function to read the entire file but doesn't have a function to read a file one line at a time.

**Python:**
```python
with open('myfile.txt', 'r') as f:
    lines = f.readlines()
# lines == ['line1\n', 'line2\n'] 
```

**Vim9Script:**
```vim
var lines = readfile("myfile.txt")
# lines == ['line1', 'line2'] 
```

*Help:* [readfile()](https://vimhelp.org/builtin.txt.html#readfile%28%29)

### Writing lines to a file
**Python:**
```python
lines = ['line1\n', 'line2\n']
with open('myfile.txt', 'w') as fh:
    fh.writelines(lines)
    
lines = ['line1', 'line2']
with open('myfile.txt', 'w') as fh:
    print(*lines, sep='\n', file=fh)
```

**Vim9Script:**
```vim
writefile(['line1', 'line2'], 'myfile.txt')
```

*Help:* [writefile()](https://vimhelp.org/builtin.txt.html#writefile%28%29)

### Appending lines to a file
**Python:**
```python
lines = ["line3\n", "line4\n"]
with open('myfile.txt', 'a') as fh:
    fh.writelines(lines)
```

**Vim9Script:**
```vim
writefile(['line3', 'line4'], 'myfile.txt', 'a')
```

*Help:* [writefile()](https://vimhelp.org/builtin.txt.html#writefile%28%29)

### Checking whether a file exists
**Python:**
```python
import os.path
if os.path.isfile('myfile.txt'):
    print("File exists")
```

**Vim9Script:**
```vim
if filereadable('myfile.txt')
  echo "File is readable"
endif
```

*Help:* [filereadable()](https://vimhelp.org/builtin.txt.html#filereadable%28%29)

### Deleting a file
**Python:**
```python
import os
os.remove('myfile.txt')
```

**Vim9Script:**
```vim
delete('myfile.txt')
```

*Help:* [remove()](https://vimhelp.org/builtin.txt.html#remove%28%29)

### Renaming a file
**Python:**
```python
import os
os.rename('myfile.txt', 'somefile.txt)
```

**Vim9Script:**
```vim
rename('myfile.txt', 'somefile.txt')
```

*Help:* [rename()](https://vimhelp.org/builtin.txt.html#rename%28%29)

### Getting the size of a file
**Python:**
```python
import os
sz = os.path.getsize('move.py')
```

**Vim9Script:**
```vim
var sz = getfsize('move.py')
```

*Help:* [getfsize()](https://vimhelp.org/builtin.txt.html#getfsize%28%29)

------------------------------------------------------------------------------

## Directory Operations

### Creating a directory
**Python:**
```python
os.mkdir('somedir')
```

**Vim9Script:**
```vim
mkdir('somedir')
```

*Help:* [mkdir()](https://vimhelp.org/builtin.txt.html#mkdir%28%29)

### Changing to a directory
**Python:**
```python
os.chdir('someotherdir')
```

**Vim9Script:**
```vim
chdir('someotherdir')
```

*Help:* [chdir()](https://vimhelp.org/builtin.txt.html#chdir%28%29)

### Getting the current directory
**Python:**
```python
dir = os.getcwd()
```

**Vim9Script:**
```vim
var dir = getcwd()
```

*Help:* [getcwd()](https://vimhelp.org/builtin.txt.html#getcwd%28%29)

### Deleting a directory
**Python:**
```python
os.rmdir('somedir')
```

**Vim9Script:**
```vim
delete('somedir', 'd')
```

*Help:* [delete()](https://vimhelp.org/builtin.txt.html#delete%28%29)

### Reading the directory contents
**Python:**
```python
import os
dir = os.scandir('.')
for f in dir:
  print(f.name)

# get extended file information
dir = os.scandir('.')
for f in dir:
  print(f.stat())
```

**Vim9Script:**
```vim
var dir = readdir('.')
for f in dir
  echo f
endfor

# get extended file information
var dir2 = readdirex('.')
for f2 in dir2
  echo f2
endfor
```

*Help:* [readdir()](https://vimhelp.org/builtin.txt.html#readdir%28%29), [readdirex()](https://vimhelp.org/builtin.txt.html#readdirex%28%29), [glob()](https://vimhelp.org/builtin.txt.html#glob%28%29)

------------------------------------------------------------------------------

## Random numbers

### Generating a random number
**Python:**
```python
import random
r = random.randint(0, 2147483647)
```

**Vim9Script:**
```vim
var r = rand()
```

*Help:* [rand()](https://vimhelp.org/builtin.txt.html#rand%28%29)

### Generating a random number from a seed
**Python:**
```python
import random
random.seed()
r = random.randint(0, 2147483647)
print(r)
```

**Vim9Script:**
```vim
var seed = srand()
var r = rand(seed)
```

*Help:* [srand()](https://vimhelp.org/builtin.txt.html#srand%28%29)

------------------------------------------------------------------------------

## Mathematical Functions

Function|Python|Vim9Script
----|------|---------
abs| `f = math.fabs(-10)` | `var f = abs(-10)`
acos| `f = math.acos(0.8)` | `var f = acos(0.8)`
asin| `f = math.asin(0.8)` | `var f = asin(0.8)`
atan| `f = math.atan(0.8)` | `var f = atan(0.8)`
atan2| `f = math.atan2(0.4, 0.8)` | `var f = atan2(0.4, 0.8)`
ceil| `f = math.ceil(1.2)` | `var f = ceil(1.2)`
cos| `f = math.cos(4)` | `var f = cos(4)`
cosh| `f = math.cosh(4)` | `var f = cosh(4)`
exp| `f = math.exp(2)` | `var f = exp(2)`
floor| `f = math.floor(1.4)` | `var f = floor(1.4)`
log| `f = math.log(12)` | `var f = log(12)`
log10| `f = math.log10(100)` | `var f = log10(100)`
mod| `f = math.fmod(4, 3)` | `var f = fmod(4, 3)`
pow| `f = math.pow(2, 3)` | `var f = pow(2, 3)`
sin| `f = math.sin(4)` | `var f = sin(4)`
sinh| `f = math.sinh(4)` | `var f = sinh(4)`
sqrt| `f = math.sqrt(9)` | `var f = sqrt(9)`
tan| `f = math.tan(4)` | `var f = tan(4)`
tanh| `f = math.tanh(4)` | `var f = tanh(4)`
trunc| `f = math.trunc(1.3)` | `var f = trunc(1.3)`

*Help:* [ceil()](https://vimhelp.org/builtin.txt.html#ceil%28%29), [abs()](https://vimhelp.org/builtin.txt.html#abs%28%29), [floor()](https://vimhelp.org/builtin.txt.html#floor%28%29), [fmod()](https://vimhelp.org/builtin.txt.html#fmod%28%29), [trunc()](https://vimhelp.org/builtin.txt.html#trunc%28%29), [exp()](https://vimhelp.org/builtin.txt.html#exp%28%29), [log()](https://vimhelp.org/builtin.txt.html#log%28%29), [log10()](https://vimhelp.org/builtin.txt.html#log10%28%29), [pow()](https://vimhelp.org/builtin.txt.html#pow%28%29), [sqrt()](https://vimhelp.org/builtin.txt.html#sqrt%28%29), [cos()](https://vimhelp.org/builtin.txt.html#cos%28%29), [sin()](https://vimhelp.org/builtin.txt.html#sin%28%29), [tan()](https://vimhelp.org/builtin.txt.html#tan%28%29), [cosh()](https://vimhelp.org/builtin.txt.html#cosh%28%29), [sinh()](https://vimhelp.org/builtin.txt.html#sinh%28%29), [tanh()](https://vimhelp.org/builtin.txt.html#tanh%28%29), [acos()](https://vimhelp.org/builtin.txt.html#acos%28%29), [asin()](https://vimhelp.org/builtin.txt.html#asin%28%29), [atan()](https://vimhelp.org/builtin.txt.html#atan%28%29), [atan2()](https://vimhelp.org/builtin.txt.html#atan2%28%29)

------------------------------------------------------------------------------

## Date/Time functions

### Get current date and time

**Python:**
```python
from datetime import datetime
d = datetime.now()
print(d.strftime("%c"))
```

**Vim9Script:**
```vim
echo strftime("%c")
```

*Help:* [strftime()](https://vimhelp.org/builtin.txt.html#strftime%28%29)

### Parse a date/time string

**Python:**
```python
from datetime import datetime
print(datetime.strptime("1997 Apr 27 11:49:23", "%Y %b %d %X"))
```

**Vim9Script:**
```vim
echo strptime("%Y %b %d %X", "1997 Apr 27 11:49:23")
```

*Help:* [strptime()](https://vimhelp.org/builtin.txt.html#strptime%28%29)

### Getting the time in seconds since epoch

**Python:**
```python
import time
print int(time.time())
```

**Vim9Script:**
```vim
echo localtime()
```

*Help:* [localtime()](https://vimhelp.org/builtin.txt.html#localtime%28%29)

### Measuring elapsed time

**Python:**
```python
import time
start_time = time.perf_counter()
sum = 1
for i in range(1000):
    sum += i
end_time = time.perf_counter()
print("Elapsed time " + str(end_time - start_time))
```

**Vim9Script:**
```vim
var start = reltime()
var sum = 0
for i in range(1000)
  sum += i
endfor
var elapsed_time = reltime(start)
echo "Elasped time" reltimefloat(elapsed_time)
```

*Help:* [reltime()](https://vimhelp.org/builtin.txt.html#reltime%28%29), [reltimestr()](https://vimhelp.org/builtin.txt.html#reltimestr%28%29), [reltimefloat()](https://vimhelp.org/builtin.txt.html#reltimefloat%28%29)

------------------------------------------------------------------------------

## External commands

### Getting the output of an external command as a string

**Python:**
```python
import subprocess
procObj = subprocess.Popen('grep class *.java',
    stdout=subprocess.PIPE,
    shell=True)
lines, err = procObj.communicate()
print(lines)
print("Error = " + str(procObj.returncode))
```

**Vim9Script:**
```vim
var lines = system('grep class *.java')
echo lines
echo $"Error = {v:shell_error}"
```

*Help:* [system()](https://vimhelp.org/builtin.txt.html#system%28%29), [v:shell_error](https://vimhelp.org/eval.txt.html#v%3ashell_error)

### Splitting the output of an external command into lines

**Python:**
```python
import subprocess
procObj = subprocess.Popen('grep class *.java',
    stdout=subprocess.PIPE,
    shell=True)
lines, err = procObj.communicate()
print("Number of matches = " + str(len(lines.splitlines())))
```

**Vim9Script:**
```vim
var lines = systemlist('grep class *.java')
echo $"Number of matches = {len(lines)}"
```

*Help:* [systemlist()](https://vimhelp.org/builtin.txt.html#systemlist%28%29)

### Sending input to an external command and getting the output

**Python:**
```python
import subprocess
procObj = subprocess.Popen('wc',
    stdout=subprocess.PIPE,
    stdin=subprocess.PIPE,
    shell=True)
lines, err = procObj.communicate("one\ntwo\n")
print(lines)
print("Error = " + str(procObj.returncode))
```

**Vim9Script:**
```vim
var lines = system('wc', "one\ntwo\n")
echo lines
echo $"Error = {v:shell_error}"
```

*Help:* [system()](https://vimhelp.org/builtin.txt.html#system%28%29)

------------------------------------------------------------------------------

## User Input/Output

### Getting input from the user

**Python:**
```python
choice = input("coffee or tea? ")
print("You selected " + choice)
```

**Vim9Script:**
```vim
var ans = input("coffee or tea? ", "tea")
echo $"You selected {ans}"
```

*Help:* [input()](https://vimhelp.org/builtin.txt.html#input%28%29), [inputlist()](https://vimhelp.org/builtin.txt.html#inputlist%28%29), [inputdialog()](https://vimhelp.org/builtin.txt.html#inputdialog%28%29)

### Getting password from the user

**Python:**
```python
import getpass
passwd = getpass.getpass("Password: ")
print("You entered " + passwd)
```

**Vim9Script:**
```vim
var passwd = inputsecret("Password: ")
echo $"You entered {passwd}"
```

*Help:* [inputsecret()](https://vimhelp.org/builtin.txt.html#inputsecret%28%29)

### Print an expression

**Python:**
```python
print("Hello World\n")
s = "vim"
print("Editor = %s" % (s))
```

**Vim9Script:**
```vim
echo "Hello World"
var s = "vim"
echo $"Editor = {s}"
```

*Help:* [:echo](https://vimhelp.org/eval.txt.html#%3Aecho), [:echon](https://vimhelp.org/eval.txt.html#%3Aechon), [echoraw()](https://vimhelp.org/builtin.txt.html#echoraw%28%29), [:echohl](https://vimhelp.org/eval.txt.html#%3Aechohl), [:echoerr](https://vimhelp.org/eval.txt.html#%3Aechoerr), [:echomsg](https://vimhelp.org/eval.txt.html#%3Aechomsg)

### Formatted Output

**Python:**
```python
name = "John"
id = 1001
print(f"Name: {name}, ID: {id}")
print("Name: {}, ID: {}".format(name, id))
print("Name: %s, ID: %d" % (name, id))
```

**Vim9Script:**
```vim
var name = "John"
var id = 1001
echo printf("Name: %s, ID: %d", name, id)
```

*Help:* [printf()](https://vimhelp.org/builtin.txt.html#printf%28%29)

------------------------------------------------------------------------------

## Environment Variables

### Getting the value of an environment variable

**Python:**
```python
import os
h = os.environ.get('HOME', '')
if h == '':
    print("HOME is not set")
else:
    print("HOME = " + h)
```

**Vim9Script:**
```vim
var h = getenv('HOME')
if h == v:null
  echo 'HOME is not set'
else
  echo $'HOME = {h}'
endif

if !exists('$HOME')
  echo 'HOME is not set'
else
  echo $'HOME = {$HOME}'
endif
```

*Help:* [getenv()](https://vimhelp.org/builtin.txt.html#getenv%28%29), [expr-env](https://vimhelp.org/eval.txt.html#expr-env), [exists()](https://vimhelp.org/builtin.txt.html#exists%28%29)

### Setting an environment variable

**Python:**
```python
import os
os.environ['FOO'] = "BAR"
```

**Vim9Script:**
```vim
setenv('FOO', 'BAR')

$FOO = 'BAR'
```

*Help:* [setenv()](https://vimhelp.org/builtin.txt.html#setenv%28%29), [:let-environment](https://vimhelp.org/eval.txt.html#%3alet-environment)

### Removing an environment variable

**Python:**
```python
import os
del os.environ['FOO']
```

**Vim9Script:**
```vim
setenv('FOO', null)

# Another method to unset an environment variable
unlet $FOO
```

*Help:* [setenv()](https://vimhelp.org/builtin.txt.html#setenv%28%29), [:unlet-environment](https://vimhelp.org/eval.txt.html#%3aunlet-environment)

### Getting all the environment variables

**Python:**
```python
import os
print(os.environ)
```

**Vim9Script:**
```vim
echo environ()
```

*Help:* [environ()](https://vimhelp.org/builtin.txt.html#environ%28%29)

------------------------------------------------------------------------------

## Command-line Arguments

### Displaying the command-line arguments

**Python:**
```python
import sys
print("Number of arguments = " + str(len(sys.argv)))
print("Arguments = " + str(sys.argv))
for arg in sys.argv:
    print(arg)
```

**Vim9Script:**
```vim
echo $"Number of arguments = {len(v:argv)}"
echo $"Arguments = {string(v:argv)}"
for arg in v:argv
  echo arg
endfor
```

*Help:* [v:argv](https://vimhelp.org/eval.txt.html#v%3aargv)

------------------------------------------------------------------------------

## Regular Expressions

### Finding whether a pattern matches a string

**Python:**
```python
import re
s = 'Test failed with error E123:'
if re.search(r'E\d+:', s):
    print('Error code found')

s = 'Test successful'
if re.search(r'E\d+:', s) is None:
    print("Test passed")
```

**Vim9Script:**
```vim
var s = 'Test failed with error E123:'
if s =~# 'E\d\+:'
  echo "Error code found"
endif

s = 'Test successful'
if s !~# 'E\d\+:'
  echo "Test passed"
endif
```

*Help:* [expr-=~](https://vimhelp.org/eval.txt.html#expr-%3d~), [expr-!~](https://vimhelp.org/eval.txt.html#expr-%21~)

### Finding the beginning or ending index of a pattern in a string

**Python:**
```python
import re
m = re.search(r'\d+', "Abc 123 Def")
if m is not None:
    idx = m.start()
    end_idx = m.end()
```

**Vim9Script:**
```vim
var idx = match("Abc 123 Def", '\d\+')
var end_idx = matchend("Abc 123 Def", '\d\+')

var l = matchstrpos("Abc 123 Def", '\d\+')
echo $"start = {l[1]}, end = {l[2]}"
```

*Help:* [match()](https://vimhelp.org/builtin.txt.html#match%28%29), [matchend()](https://vimhelp.org/builtin.txt.html#matchend%28%29), [matchstrpos()](https://vimhelp.org/builtin.txt.html#matchstrpos%28%29)

### Getting matching substring using a pattern

**Python:**
```python
import re
m = re.search(r'\d+', "Abc 123 Def")
if m is not None:
    s = m.group(0)
    print s
```

**Vim9Script:**
```vim
var s = matchstr("Abc 123 Def", '\d\+')
```

*Help:* [matchstr()](https://vimhelp.org/builtin.txt.html#matchstr%28%29)

### Getting multiple matches using a pattern

**Python:**
```python
import re
m = re.match(r'(\w+) (\w+) (\w+)', "foo bar baz")
if m is not None:
    print("Full match: " + m.group(0))
    print("1: " + m.group(1) + " 2: " + m.group(2) + " 3: " + m.group(3))
```

**Vim9Script:**
```vim
var list = matchlist("foo bar baz", '\(\w\+\) \(\w\+\) \(\w\+\)')
echo "Full match:" list[0]
echo "1:" list[1] "2:" list[2] "3:" list[3]
```

*Help:* [matchlist()](https://vimhelp.org/builtin.txt.html#matchlist%28%29)

### Substituting text using a pattern

**Python:**
```python
import re
s = re.sub(r'bar', r'baz', "foo bar")
print(s)
```

**Vim9Script:**
```vim
var s = substitute("foo bar", 'bar', 'baz', '')
echo s
```

*Help:* [substitute()](https://vimhelp.org/builtin.txt.html#substitute%28%29)

### Using a function to get the replacement string

**Python:**
```python
import re
def Dashrepl(m):
  if m.group(0) == '-':
    return ' '
  else:
    return '-'
s = re.sub('-{1,2}', Dashrepl, 'pro----gram-files')
print(s)
```

**Vim9Script:**
```vim
def Dashrepl(m: list<string>): string
  if m[0] == '-'
    return ' '
  else
    return '-'
  endif
enddef
var s = substitute("pro----gram-files", '-\{1,2}', function('Dashrepl'), 'g')
echo s
```

*Help:* [substitute()](https://vimhelp.org/builtin.txt.html#substitute%28%29)

### Regular expression comparison

Note that the below table contains only the regular expressions that are present in both Python and Vim.

What|Python|Vim
----|------|---
single character| `.` | `.`
start of string| `^` | `^`
end of string| `$` | `$`
0 or more matches| `*` | `*`
1 or more matches| `+` | `\+`
0 or 1 match| `?` | `\?`
non-greedy match| `*?` | `\{-}`
fixed matches| `{n}` | `\{n}`
m to n matches| `{m,n}` | `\{m,n}`
m to n non-greedy matches| `{m,n}?` | `\{-m,n}`
character class| `[...]` | `[...]`
negated character class| `[^...]` | `[^...]`
range| `[a-z]` | `[a-z]`
either-or branch| `\|` | `\\|`
capturing group| `(...)` | `\(...\)`
non-capturing match| `(?:...)` | `\%(...\)`
positive look-ahead| `(?=...)` | `\(...\)\@=`
negative look-ahead| `(?!...)` | `\(...\)\@!`
positive look-behind| `(?<=...)` | `\(...\)\@<=`
negative look-behind| `(?<!...)` | `\(...\)\@<!`
start of word| `\b` | `\<`
end of word| `\b` | `\>`
digit| `\d` | `\d`
non-digit| `\D` | `\D`
whitespace| `\s` | `\s`
non-whitespace| `\S` | `\S`
word character| `\w` | `\w`
non-word character| `\W` | `\W`
ignore case| `(?i)` | `\c`

*Help:* [pattern](https://vimhelp.org/pattern.txt.html#pattern)

------------------------------------------------------------------------------

## Binary Data

### Storing binary data in a variable

**Python:**
```python
data = bytearray(b'\x12\xF6\xAB\xFF\x49\xC0\x88\x3A\xE2\xC1\x42\xAA')
print(data)
print(data[0:4])
```

**Vim9Script:**
```vim
var data = 0z12F6ABFF.49C0883A.E2C142AA
echo data
echo data[0 : 3]
```

*Help:* [blob](https://vimhelp.org/eval.txt.html#blob)

### Manipulating binary data stored in a variable

**Python:**
```python
data = bytearray(b'')
data.append(0xC2)
data += b'\xB3\xF7\xA5'
del data[2]
print(data)
```

**Vim9Script:**
```vim
var data = 0z
data[0] = 0xC2
data += 0zB3F7A5
remove(data, 2)
echo data
```

*Help:* [blob-index](https://vimhelp.org/eval.txt.html#blob-index), [blob-modification](https://vimhelp.org/eval.txt.html#blob-modification)

### Converting a List of numbers to binary data and vice versa

**Python:**
```python
l = [11, 12, 14]
data = bytearray(l)
print(data)

data = bytearray(b'\xDE\xAD\xBE\xEF')
l = list(data)
print(l)
```

**Vim9Script:**
```vim
var l = [11, 12, 14]
var data = list2blob(l)
echo data

data = 0zDEADBEEF
l = blob2list(data)
echo l
```

*Help:* [list2blob()](https://vimhelp.org/builtin.txt.html#list2blob%28%29), [blob2list()](https://vimhelp.org/builtin.txt.html#blob2list%28%29)

### Reading and writing binary data from a file

**Python:**
```python
with open("datafile.bin", "rb") as bin_fh:
    data = bytearray(bin_fh.read())

with open("data2.bin", "wb") as bin_fh:
    bin_fh.write(data)
```

**Vim9Script:**
```vim
var data = readblob('datafile.bin')
writefile(data, 'data2.bin')
```

*Help:* [readblob()](https://vimhelp.org/builtin.txt.html#readblob%28%29), [writefile()](https://vimhelp.org/builtin.txt.html#writefile%28%29)

------------------------------------------------------------------------------

## Timers

### One-shot Timer

**Python:**
```python
import threading
def TimerCallback(ctx):
    print("Timer callback, context = " + ctx)

timer = threading.Timer(5, TimerCallback, args=["green"])
timer.start()
```

**Vim9Script:**
```vim
def TimerCallback(ctx: string, tid: number)
  echo $"Timer callback, context = {ctx}, id = {tid}"
enddef

# Run a function after 5 seconds
var timer_id = timer_start(5 * 1000, function('TimerCallback', ["green"]))
```

*Help:* [timer_start()](https://vimhelp.org/builtin.txt.html#timer_start%28%29)

### Periodic Timer

**Python:**
```python
import threading
def TimerCallback():
    print("Timer callback")
    threading.Timer(5, TimerCallback).start()

# run a function every 5 seconds (approximately)
timer = threading.Timer(5, TimerCallback)
timer.start()
```

**Vim9Script:**
```vim
def TimerCallback(tid: number)
  echo "Timer callback"
enddef

# run a function every 5 seconds periodically
var timer_id = timer_start(5 * 1000, function('TimerCallback'), {'repeat': -1})
```

*Help:* [timer](https://vimhelp.org/eval.txt.html#timer)

### Stopping a timer

**Python:**
```python
import threading
def TimerCallback():
    print("Timer callback")

timer = threading.Timer(1, TimerCallback)
timer.start()
timer.cancel()
```

**Vim9Script:**
```vim
def TimerCallback(tid: number)
  echo "Timer callback"
enddef

# start a timer and then stop it
var timer_id = timer_start(1000, 'TimerCallback')
timer_stop(timer_id)

# to stop all the timers
timer_stopall()
```

*Help:* [timer_start()](https://vimhelp.org/builtin.txt.html#timer_start%28%29)

### Sleeping for a specified number of seconds

**Python:**
```python
import time
time.sleep(5)
time.sleep(0.2)
```

**Vim9Script:**
```vim
# sleep for 5 seconds
sleep 5
# sleep for 200 milliseconds
sleep 200m
```

*Help:* [:sleep](https://vimhelp.org/various.txt.html#%3asleep)

------------------------------------------------------------------------------

## JSON encoder and decoder

**Python:**
```python
import json
v = ['foo', {'a' : 3}, True]
# encode data to JSON
str = json.dumps(v)
# decode data from JSON
x = json.loads(str)
```

**Vim9Script:**
```vim
var v = ['foo', {'a': 3}, true]
# encode data to JSON
var str = v->json_encode()
echo str
# decode data from JSON
var x = str->json_decode()
echo x
```

*Help:* [json_encode()](https://vimhelp.org/builtin.txt.html#json_encode%28%29), [json_decode()](https://vimhelp.org/builtin.txt.html#json_decode%28%29), [js_encode()](https://vimhelp.org/builtin.txt.html#js_encode%28%29), [js_decode()](https://vimhelp.org/builtin.txt.html#js_decode%28%29)

------------------------------------------------------------------------------

## Network Sockets

**Python:**
```python
import requests
# Get a web page from http://httpbin.org
def Display_Page():
  r = requests.get('http://httpbin.org/')
  if r.status_code == requests.codes.ok:
    # display the received header and contents
    print(r.headers)
    print(r.text)
  else:
    print("Error: Failed to open URL")
Display_Page()
```

**Vim9Script:**
```vim
g:rcvd_data = []

# channel data callback function. Called for every received line.
def Chan_data_cb(ch: channel, msg: string)
  add(g:rcvd_data, msg)
enddef

# channel close callback function.
def Chan_close_cb(ch: channel)
  echo g:rcvd_data
enddef

def Display_Page()
  var addr = "httpbin.org:80"
  var ch_opt = {}
  # data received is processed one line at a time
  ch_opt.mode = 'nl'
  ch_opt.waittime = -1
  ch_opt.drop = "never"
  ch_opt.callback = function('Chan_data_cb')
  ch_opt.close_cb = function('Chan_close_cb')
  # open the channel
  var ch = ch_open(addr, ch_opt)
  if ch_status(ch) != "open"
    echomsg $"Failed to open channel, status = {ch_status(ch)}"
    return
  endif
  # send a http request
  ch_sendraw(ch, "GET / HTTP/1.0\nHost: httpbin.org\n\n")
enddef

Display_Page()
```

*Help:* [job-channel-overview](https://vimhelp.org/channel.txt.html#job-channel-overview), [ch_open()](https://vimhelp.org/channel.txt.html#ch_open%28%29), [ch_status()](https://vimhelp.org/channel.txt.html#ch_status%28%29), [ch_sendraw()](https://vimhelp.org/channel.txt.html#ch_sendraw%28%29), [ch_close()](https://vimhelp.org/channel.txt.html#ch_close%28%29)

------------------------------------------------------------------------------

## Background Processes

### Starting a background process and communicating with it

The 'bc' calculator utility is used in the below example for illustrative purposes only.

**Python:**
```python
import subprocess
procObj = subprocess.Popen('bc',
    stdin=subprocess.PIPE,
    stdout=subprocess.PIPE,
    shell=True)
lines, err = procObj.communicate("12 * 6\n")
print("Result = " + lines)
print("Exitcode = " + str(procObj.returncode))
```

**Vim9Script:**
```vim
var job = job_start('bc')
if job_status(job) != "run"
  echo "Failed to start bc"
else
  var output = ch_evalraw(job, "6 * 12\n")
  echo $"Result = {output}"
  job_stop(job, "kill")
  var info = job_info(job)
  echo $"Exitcode = {info.exitval}"
endif
```

*Help:* [job](https://vimhelp.org/channel.txt.html#job), [job_start()](https://vimhelp.org/channel.txt.html#job_start%28%29), [job_status()](https://vimhelp.org/channel.txt.html#job_status%28%29), [job_stop()](https://vimhelp.org/channel.txt.html#job_stop%28%29), [job_info()](https://vimhelp.org/channel.txt.html#job_info%28%29)

------------------------------------------------------------------------------

## Unit Tests

**Python:**
```python
import unittest

class TestDemoMethods(unittest.TestCase):
    def test_abc(self):
        self.assertEqual('FOO'.lower(), 'foo')
        self.assertNotEqual(1, 2)
        self.assertTrue('foo' == 'foo')
        self.assertFalse('FOO' == 'foo')
        self.assertIsNot('foo', 1)
        self.assertRegex('ab123xy', '\d+')
        self.assertNotRegex('abcd', '\d+')
        with self.assertRaises(TypeError):
            'a:b'.split(2)

unittest.main()
```

**Vim9Script:**
```vim
v:errors = []
assert_equal(tolower('FOO'), 'foo')
assert_notequal(1, 2)
assert_true('foo' == 'foo')
assert_false('FOO' == 'foo')
assert_fails('echo split("a:b", [])', 'E730:')
assert_match('\d\+', 'ab123xy')
assert_notmatch('\d\+', 'abcd')
if len(v:errors) == 0
    echo "Test passed"
else
    echo $"Test failed: {v:errors}"
endif
```

*Help:* [testing](https://vimhelp.org/testing.txt.html#testing), [v:errors](https://vimhelp.org/eval.txt.html#errors-variable)
