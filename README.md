![cx logo](https://user-images.githubusercontent.com/26845312/32426758-2a4bbb00-c282-11e7-858e-a1eaf3ea92f3.png)

# CX Programming Language

CX is a general purpose, interpreted and compiled programming
language, with a very strict type system and a syntax
similar to Golang's. CX provides a new programming paradigm based on
the concept of affordances, where the user can ask the programming
language at runtime what can be done with a CX object (functions,
expressions, packages, etc.), and interactively or automatically choose
one of the affordances to be applied.

# Installation

First, make sure that you have Go installed by running `go
version`. It should output something similar to

```
go version go1.8.3 darwin/amd64
```

Go should also be properly configured (you can read the installation
instructions by clicking [here](https://golang.org/doc/install).

Download skycoin

```
go get https://github.com/skycoin/skycoin/...
```

Clone this repository

# CX Tutorial

In the following sections, the reader can find a short tutorial on how
to use the main features of the language. It can be used as introductory
material for people with no experience in programming as some sections
are explained as if this was the main audience. Nevertheless, the true
purpose of the tutorial is to demonstrate all the features that the
language currently supports.

Feel free to [create an issue](https://github.com/skycoin/cx/issues)
requesting a better explanation of a feature.

# Hello World

Do you want to know how CX looks? This is how you print "Hello World!"
in a terminal:

```
package main

func main () () {
	str.print("Hello World!")
}
```

Every CX program must have at least a *main* package, and a *main*
function. As mentioned before, CX has a very stricty type system,
where functions can only be associated with a single type
signature (i.e., polymorphism is not allowed in CX). As a consequence,
if we want to print a string, as in the example above, we have to call
*str*'s print function, where *str* is a package containing string
related functions.

# Comments

Before continuing with more complex subjects,

Some of the code snippets that follow have blocks of text that are
prefixed by double slashes (//). Double slashes inform the CX parser
that what

Some of the code snippets that follow have comments in them, i.e.,
blocks of text that are not actually "run" by the CX compiler or
interpreter. Just like in C, Golang and many other programming
languages, single line comments are created by placing double slashes
(//) before the text being commented. For example:

```
// Example of summing two 32 bit integers in CX

i32.add(3, 4)       // This will be ignored

// End of the program
```

Mult-line comments are opened by writing slash-asterisk (/\*), and are
closed by writing asterisk-slash (\*/).

```
/* This code won't be executed
str.print("Hello world!")
*/
```

# Data

Every programming language is designed to manipulate some kind of data
using some kind of process. Let's first have a look at the simplest
kind of data that we can create in CX, and then move on to slightly more
complex data structures.

## Atomic Data
### Booleans

Booleans can be either *true* or *false*, and they are mainly used to
control the flow of a program. As an example, let's print both
possible values to the terminal:

```
bool.print(true)
bool.print(false)
```

### Integers

CX can work with either 32 or 64 bit integers. The types themselves
are called *i32* and *i64*  respectively. Any number without decimal
points are considered to be *i32*, e.g. *5* or *12*.

Unlike i32 numbers, the programmer needs to explicitly tell CX when an
i64 number is required. For example, to print the *i64* number "15",
you'd need to write:

```
i64.print(i32.i64(15))
```

### Floats

Floating-point numbers come in two sizes, just like integers: 32 and
64 bits. 32-bit floats are named *f32*s, while 64-bit floats are named
*f64*s. Similar to *i32*, the programmer does not need any explicit
casting; CX simply regards any number that has a decimal point as an
*f32*. In the case of *f64* numbers, the programmer needs to cast an
*f32* number to *f64* before being passed as an argument to a
function, for example:

```
i64.add(i32.i64(30.0), i32.i64(20.0))
```

### Bytes

Bytes can hold any number from 0 to 255. The programmer can create a
byte number by casting an *i32* to *byte*:

```
i32.byte(255)
```

### Strings

Strings are internally represented as array of bytes. CX's parser
recognizes any chain of characters enclosed by a pair of double quotes
(") as a string:

```
"I'm a string"
"I'm
    also a
            string"
```

A character string in CX is said to be of type *str*.

## Arrays

Until this point, all data types that we have mentioned have been
"atomic," which means that they hold only one piece of information
(except strings, that are actually byte arrays, but they can be seen
as a single unit of information too).

An array is a collection of atomics, where every element
contained in an array must be of the same type.

CX arrays behave more like vectors in other programming languages,
where they can be resized by adding new elements.

A programmer can create arrays of each of the atomic types by writing
"[]" followed by the desired type and a list of elements that
initialize the array, enclosed in curly braces. For example:

```
[]bool{true, false, true}
[]byte{3, 2}
[]i32{0, 1, 2}
[]i64{7, 7, 7}
[]f32{3.5, 1.2, 8.9}
[]f64{0.3}
[]str{"hello", "world"}
```

As can be noted, numbers in *[]i64* and *[]f64* do not need to be
cast explicitly. The reason behind this is that they are already being
explicitly cast: *[]i64* and *[]f64* are telling CX that every element
is of that type.

There are a number of native functions associated to array types. For
example, to obtain the number of elements in an *[]i32*, we can use
*[]i32.len*:

```
i32.print([]i32.len([]i32{0, 1, 2})
```

We can read specific elements from arrays by using *[]xxx.read*, write
new elements to arrays at a particular index by using *[]xxx.write*, concatenate two arrays of
the same type by using *[]xxx.concat*, append new values to an array
by using *[]xxx.append*, and we can create copies of arrays by using
*[]xxx.copy*.

```
[]i32.read([]i32{10, 20, 30}, 0) // returns 10
[]f32.write([]f32{3.3, 4.4, 5.5}, 1, 10.10)  // returns []f32{3.3, 10.10, 5.5}
[]str.concat([]str{"hi"}, []str{"bye"}) // returns []str{"hi", "bye"}
[]f64.append([]f64{1.1}, i32.i64(2.2)) // returns []f64{1.1, 2.2}
[]bool.copy(toArray, []bool{true, false})
```

The set of array *copy* functions are meant to receive a variable
(first argument) to copy to the array (second argument). These *copy*
functions won't raise an error if literals are sent as their first
argument, but it won't be of any use for the programmer, as the new
copy of the array will be lost. You can create new and empty arrays by
using the *make* array functions:

```
empty := []i32.make(3)
[]i32.copy(empty, []i32{1, 2, 3})
```

In the example above, *empty* is a variable that holds an empty array
of length 3. We'll learn more about variables in the next section.

As a final note, CX also provides us with functions to cast arrays of one type to
another, for example: *[]f32.[]i32*, *[]i64.[]f64*, etc.

## Variables

Passing literal values to functions give us already a lot of power,
but imagine that we need to pass a 5 string array to three different
functions. In order to avoid writing the same array three times, we
can make a variable hold the value for us, and then pass the variable
to the three different functions:

```
var names []str = []str{"Edward", "Daniel", "Melissa", "Roger", "Ron"}

[]str.print(names)
notify(names)
saveToDatabase(names)
```

In the example above, the variable *names* is declared and immediately
initialized with the string array literal, but the variable could have
been declared and not explicitly initialized. Another option is to use a short form where the
*var* keyword is skipped:

```
var notInitilazed i32
short := 1.1
```

Internally, variables declared using either the long or the short forms
are represented the same. The additional syntax is there only to provide
the programmer a way to inform the reader that that variable is going
to be used later on.

Just like in Golang, a variable that is being declared and initialized
without the *var* keyboard must be assigned using a colon and an equal
symbol (:=).

Finally, as mentioned above, a variable can be declared and not
*explicitly* initialized. Unlike in languages like C, where a variable
can end up pointing to garbage in memory, every variable in CX is
implicitly initialized to its zero value (unless explicitly
initialized to something else, of course). Numerical variables are
initialized to 0 or 0.0, booleans are initialized to false, strings
are initialized to an empty string (""), and arrays are initialized to
an empty array.

### Local Variables

Variables that are declared inside a function are said to be "local."
This means that other functions do not have access to this variable:

```
func outside () () {
   var greeting str = "hello"
}
func main () () {
  str.print(greeting)
}
```

In this example, the *main* function does not have access to the
*greeting* variable declared in the *outside* function. *greeting* is
a local variable in *outside* function's scope.

Another thing to have in mind is that local variables are not shared
in different calls to the same function. For example:

```
func recur (num i32) () {
  state := i32.add(num, 1)
  if i32.eq(state, 3) {
    return
  } else {
    recur(state)
  }
}

func main () () {
   recur(1)
}
```

In the example above we can see the example of a recursive function: a
function that calls itself. The *main* function calls *recur* and
sends 1 as its argument. *recur* declares a variable called state,
which is defined as the sum of its only argument and 1. If *state* is
equal to 3, the function returns, but if it isn't, it calls itself,
sending its *state* variable as an argument. In this next call to
recur, what's *state* value going to be? Is it going to be 2, the
value obtained in the previous call? No, each function call in CX is
associated with its own "state." The correct answer is 3, which is
obtained by adding 1 to 2, the argument sent to this new call to *recur*.

### Global Variables

What can we do if we want state to be shared among several function
calls? We can use global variables. Global variables are declared
outside of any function declaration, and they must be declared before
any function that plans to use it. If the programmer wants a function
to use a global variable that is declared after a function
declaration, CX should either raise an error or declare a local
variable with that name, depending on the context in which the
variable is trying to be used (being sent as an argument to another
function or assigning a new value to the variable).

```
var counter i32

func inc () () {
  counter = i32.add(counter, 1)
}

func main () () {
  i32.print(counter)
  inc()
  i32.print(counter)
}
```

In the example above, *counter* is defined as a global variable, which
means that any function declared after it, has access to its
value. The *main* function starts its block of expressions by printing
*counter*'s value, which is 0. Then a call to *inc* is performed,
which increases *counter*'s value by 1. *main* then prints again the
value of counter, which should now be 2.

## Structs

We can create groups of variables by using *struct*s. *struct*s are
useful for representing more complex data abstractions, where
different data types and data encapsulations are needed. For example,
if the programmer needs to represent a *Point*, it can be defined in
the following way:

```
type Point struct {
  x i32
  y i32
}
```

The name of the *struct* needs to be surrounded by the keywords *type*
and *struct*. The *struct*'s *fields* are enclosed by curly braces,
and each field is defined by a name or identifier and a type assigned
to that identifier.

Any basic type can be used as the type of a *struct* *field*:

```
type Student struct {
  name str
  age i32
  height f32
  grades []f32
}
```

*struct*s can also serve as field types in other *struct*s, but we
need to remember to declare the *struct*s in the correct order:

```
type Color struct {
  r i32
  g i32
  b i32
}

type Point struct {
  x i32
  y i32
}

type Shape struct {
  color Color
  vertices []Point
}
```

If we had declared *Shape* before *Color* or *Point*, CX would raise
an error telling us that type "Color" or type "Point" is not defined.

As can be noted, as soon as we declare a new type using a *struct*, we
automatically have access to another type: arrays of that type of
*struct*s. CX not only creates this additional type for us, but a set
of functions to manipulate this new array type. Remember, CX is very
strict regarding its type system, so if we want to know what's the
length of an array of *Point*s, we'd need to call *[]Point.len* to
find out:

```
var color Color
color.r = 31
color.g = 23
color.b = 131

points := []Point.make(3)

myShape := new Shape{
  color: color,
  vertices: points
}

[]Point.write(myShape.vertices, 0, new Point{x: 1, y: 2})
[]Point.write(myShape.vertices, 1, new Point{x: 3, y: 5})
[]Point.write(myShape.vertices, 2, new Point{x: 2, y: 7})
```

Woa woa! A lot is happening in the example above. Let's analyze this
step by step. First, we can notice that we can now declare variables
of custom types (*struct*s): we create a *Color* variable named
*color*. Similarly to many other programming languages that are
capable of declaring C structs, we can access the *struct* fields by
using a dot following the variable name, and we tell CX to assign
different values to the r, g, b fields of the *Color* type.

Then we can see how we declare and initialize the *points* variable
by assigning the result of the function call *[]Point.make(3)*. CX
also created a *make* function to initialize arrays of the *Point*
type. Each of the *Point*s in this newly created array are initialized
to its 0 form, i.e., they are *Point* *struct* instances with *x = 0*
and *y = 0*.

Now, in order to create a *Shape* instance, we use the keyword "new"
followed by the name of the *struct* we want to create. This is an
alternative to using the "var name type" form. The advantage to this
new form (pun intended) is that we can use the created literal as an
argument to a function call. Anyway, by using the *new Struct* form,
we can also directly specify what are going to be the values for the
*struct* instance fields. In this case, the instance's field color is
set to the color variable defined above, and the vertices field is set
to the points array, also defined above.

Lastly, we can see how the empty points are re-assigned by using the
*[]Point.write* function. *myShape.vertices* is sent to *[]Point.write* as the
first argument, which means that we want to write a new value of type
*Point* in the *vertices* array. Each of the three calls writes a new
*Point* literal to each of the available indexes (0, 1 and 3).

# Expressions

An expression consists of a function to be
called, a set of arguments that are sent to the function, and a set of
receiving variables which will hold the outputs of the function being
called. Everything inside a CX function is an expression or is converted to
expressions.

```
foo := i32.mul(3, 5)
```

In the example above, we are telling CX to do 3 times 5 and to store
the result in *foo*. *i32.mul* is the function to be called, 3 and 5
are its arguments, and *foo* is the variable that will store the
output. As in Golang, functions in CX can return more than one value:

```
seconds, minutes, hours := getTime()
```

Arguments to function calls can be other expressions:

```
result := i32.mul(i32.add(5, 2), i32.sub(3, 1))
```

When stating multiple receiving variables, you can provide different
expressions to be assigned to each variable, for example:

```
res1, res2 := i32.add(5, 3), i32.sub(10, 7)
i32.print(res1)
i32.print(res2)
```

After executing the example above, 8 will be printed followed by 3.

# Flow Control

Like in most programming languages (if not all, excluding perhaps some
[esoteric languages](https://en.wikipedia.org/wiki/Esoteric_programming_language)),
programs are executed from top to bottom.

We can control CX's normal flow by using *flow control statements*,
which are discussed below.

## If and if/else

The first *flow control statement* is *if*, which has the capability
of ignoring execution of a number of expressions.

```
if false {
  str.print("This will never print")
}
```

In the example above, we are asking "if x", where x is a predicate
that is evaluated to determine if it's either true or false. If the
predicate is true, then the expressions enclosed in curly brackets are
executed, and are ignored if otherwise. In this case, *x = false*, so
the expression won't be executed. Predicates can be either booleans,
or variables or expressions that evaluate to booleans:


```
foo := true
if foo {
  str.print("This will print")
}

if or(true, and(false, false) {
  str.print("This will also print")
}
```

If statements can be nested:

```
if true {
  if false {
    str.print("This won't print")
  }
  if true {
    str.print("This will print")
  }
}
```

As in other programming languages, you can also create an *else*
clause. This *else* clause will execute if the predicate evaluates to
false.

```
if false {
  str.print("This won't print")
} else {
  str.print("But this will!")  
}
```

These if/else statements can also be nested:

```
if false {
  str.print("This won't print")
  } else {
  if true {
    if false {
        str.print("This won't print")
      } else {
        str.print("This will print")
      }
    }
}
```

## For Loop

Many programming languages provide several looping constructs, like
while, do-while, for-each, for, etc. CX followed the same strategy as
Golang, and only provides a *for* loop. First, let's have a look at an
infinite loop:

```
for true {
    str.print("This will print forever")
}
```

In this form, the *for* loop behaves similarly to an if statement: it
can receive a single predicate, which can be either a boolean, or a
variable or expression that evaluates to a boolean. For example:

```
for or(true, and(false, false) {
  str.print("This will also print forever")
}
```

Using this *for* form, we can create a loop that prints the numbers
from 0 to 10 like this:

```
continue := true
c := 0
for continue {
  if i32.gt(c, 10) {
    continue = false
  }
  i32.print(c)
  c = i32.add(c, 1)
}
```

It doesn't look nice, right? To make it look a bit better, we can use
the second *for* form:

```
for c := 0; i32.lteq(c, 10); c = i32.add(c, 1) {
  i32.print(c)
}
```

Much better. This form is the same as the form provided by many other
programming languages like C. The first part declares and initializes
a variable that will usually serve as a counter, the second part has a
predicate, and the last part is usually used as a counter updater. As
in these other similar programming languages, the first and the third
parts can actually have any expression you want, but they are usually
used initialize counters and update counters, respectively.

## Go-to

The last flow control structure is *go-to*. *go-to*s are used to make
a CX program jump directly to a labelled expression.

```
	goto label4
label3:
	str.print("This should never be reached")
label4:
	str.print("This should be printed")
```

In the example above, the statement "goto label4" will make CX
directly jump to the expression labelled as "label4." This flow
control statement can be combined with other flow control statements
to create complex programs. The downside with *go-to*s is usually that
programs become harder to read, but they can become very powerful in the
correct hands.

As mentioned before, everything in a CX function is actually an
expression. *go-to* statements are not an expression. Labels are
actually part of expressions, and the "goto label" statements are
actually calls to a function called *goTo*.

```
	goTo("label2")
label1:
	str.print("this should never be reached")
label2:
	str.print("this should be printed")
```

# Functions

We have already seen some examples of function calls, and an example
of a function declaration: the *main* function. We can define other
custom functions, though.

```
package main

var PI f32 = 3.14159

func circleArea (radius f32) (area f32) {
	f32.mul(f32.mul(radius, radius), PI)
}
```

In the example above, we are creating a new function that will be
contained in the *main* package. This function is called *circleArea*,
and has one input and one output parameter. If we analyze this
function, we can realize that it calculates the area of a circle,
given its radius. Once defined, it can be called in other functions
that are defined below it, such as the *main* function:

```
func main () () {
  area := circleArea(i32.f32(2))
  f32.print(area)
}
```

# Packages

Packages are a useful feature to encapsulate functions, structs and global
variables. We have been explicitly working with a single package since the
beginning of this tutorial: *main*. But we have also used functions in
other packages like *str*, *i32* and *f32*.

Functions from packages like *str* and *i32* are all implicitly imported, which
means that you can start using them right away. The reason behind this
is that these functions are so common that they could be
considered as part of the language's core library of functions.

Big projects usually need to be divided into several
packages. Depending on the programming language, these "packages"
can be called modules, namespaces, vocabularies, etc., but the idea
is always the same: to encapsulate. Encapsulation is very important,
because it allows the programmer to create groups of related
identifiers (variables, types, functions) that won't create conflicts
with other identifiers named the same, but that have a different
purpose. For example, in CX we have several versions of *print*, but
they have somewhat different purposes. Indeed, all of them will print
stuff in the terminal, but *str.print* will print a string,
*i32.print* will print a 32 bit integer, and so on. We could even have
a situation where the functions need to be named the same, and they
even receive the same type of argument, but do entirely different
things. For example, *printer.send* and *fax.send* could both receive
strings, but the former sends the string to a printer and the second
to a fax (if you don't know what a fax is, you can read about them in
[here](https://en.wikipedia.org/wiki/Fax)).

## Creating and Importing Packages

"How can I create a package in CX?" you might be wondering by now.

```
package myPackage
/*
  Functions, structs and globals are placed here
*/
```

You just need to write the keyword *package* followed by the name you
want to give to your package. Unlike Golang, you can have multiple
packages in a single source file and CX won't complain about it
(although you are encouraged to place different packages in different
files). Whenever CX reads "package something", every CX statement that
follows will be attached to the "something" package, and this
behaviour will continue until "package somethingElse" is
encountered. Let's create a Math package:

```
package Math

var PI f32 = 3.14159

func double (num f32) (double f32) {
    double = f32.mul(num, 2)
}
```

This starts looking promising, doesn't it? But how can we start using
this package in other packages? You have to *import* it.

```
package main
import "Math"

func main () () {
    i32.print(Math.PI)
    i32.print(Math.double(Math.PI))
}
```

If we don't *import Math*, CX will raise an error telling us that the
module *Math* is not being imported or does not exist.

# Debugging

Whenever an error is raised in CX, a read-eval-print loop (REPL) will
start, where the programmer can try to fix any errors with the
program. In the REPL, the programmer can modify functions, print the
call stack, print the program structure, and many other useful
things.

```
package main

func main () () {
	var foo i32 = 10
	var bar f32 = 5.5
	
	i32.div(1, 0)
}
```

In this example, we start adding some variables and then we try to
divide two numbers: 1 by 0. If we run this program, this will be
printed:

```
Call's State:
foo:		10
bar:		5.5

i32.div() Arguments:
0: 1
1: 0

7: i32.div: Division by 0
CX REPL
More information about CX is available at http://cx.skycoin.net/

* 
```

The "Call's State" section tells us what are the values of the
variables in the current call. In this case, we have two variables:
foo and bar, with 10 and 5.5 as their values, respectively. Next, CX
tells us the arguments that were sent to the expression raising the
error, which are 1 (argument #0) and 0 (argument #1). A description of
the error is also provided: CX tells us that a "Division by 0" was
raised, and it was caused by *i32.div* at line #7. Lastly, we can see
that the CX REPL starts, where the programmer can enter some commands
to try to fix the error.

## Halt

Sometimes we want to *halt* a program's execution: maybe because we
want to check what's the current call's state, maybe because we want
to see if the program's flow is reaching somewhere, maybe simply
because it's fun. Whatever the reason might be, CX provides us with a
very helpful function, which is curiously named "halt."

```
for true {
	str.print("Enter a number greater than 0: ")
	value := i32.read()

	if i32.gt(value, 0) {
		str.print("Good, good.")
	} else {
		halt("The number was not greater than 0.")
	}
}
```

In this example, CX enters an infinite loop using *for true*. The user
is asked to enter a number greater than 0, and this value is *read*
using the *i32.read* function. If this value is greater than 0, the
program congratulates the user, and if it is not greater than 0, the
program halts. As you can see, halt receives a string as it's
argument, which serves as an error message for the user.

## Unit Testing

It is a good idea to create *test* files, which contain code that
makes sure that your defined functions are behaving correctly. For
example, let's assume that we have a function that converts an integer
from 0 to 3 to it's corresponding "word name" (for example, 3 =>
"three").

```
func toWord (num i32) (name str) {
	if i32.eq(0, num) {
		name = "zero"
		return
	}
	if i32.eq(1, num) {
		name = "one"
		return
	}
	if i32.eq(2, num) {
		name = "two"
		return
	}
	if i32.eq(3, num) {
		name = "three"
		return
	}
	name = "error"
	return
}
```

We could obviously create some function calls and make sure that the
results are as expected:

```
str.print(toWord(0))
str.print(toWord(1))
str.print(toWord(2))
str.print(toWord(3))
str.print(toWord(4))
```

This solution can work for some time, but it's going to get too
complicated when your program reaches dozens of functions. A better
solution is to use CX's *test* package:

```
test.start()
test.str(toWord(0), "zero", "0 failed")
test.str(toWord(1), "one", "0 failed")
test.str(toWord(2), "two", "0 failed")
test.str(toWord(3), "three", "0 failed")
test.str(toWord(4), "error", "0 failed")
test.stop()
```

*test.start* tells CX that unit testing will start, and that
 errors should only be printed to the user instead of halting the
 program. *test.str* receives three arguments: the first two can be
 anything that returns a string, and the third one is an error message
 that will be displayed to the user in case the test fails. In order
 for the test to succeed, the evaluations of the two first arguments
 must be the same.

Now let's imagine that we have a function that needs to return an
error with a particular set of arguments. How can we test for such a
case? The solution is to use *test.error*.

```
i32.div(10, 0)
test.error("i32.div did not raise a division by 0 error")
```

Another feature that *test.start* provides is that CX becomes aware of
the raised errors. In the example above, i32.div(10, 0) *must* raise
an error (if it doesn't, that's an error). After evaluating the bugged
expression, we call *test.error*. *test.error* will raise an error if
an error was not raised by the preceding expression, and the string
provided as its first argument will be shown to the user.

The *test* package has test functions for each of CX's basic types. If
you want to see a complete example, you can see CX's unit tests
[here](https://github.com/skycoin/cx/blob/master/tests/test.cx) (that
is, you're seeing how we use CX to test that CX is working correctly).

# Affordances

If we create a CX function, what can we do with it? We can call it, we
can add more expressions to it, we can add more input and output
parameters, we can remove them, we can change its name, we can remove
the function entirely... These are called affordances in CX, and they
help us achieve meta-programming: programs that can get themselves
modified.

CX applies the affordance paradigm by using its affordance system and
inference engine. The affordance system can determine everything that
can be done to an object, and everything that that object can do to
its surroundings. The inference system filters these affordances
according to certain criteria.

As this is a complex subject in CX, let's go step by step. First, we
need to tell CX somehow what element is our target, i.e., what element
we want to get affordances of. To do this, we need to create a
*target*:

```
target := ->{
  pkg(main) fn(double) exp(multiplication)
}
```

Everything contained in *->{...}* is practically a different
mini-language, but it tries to resemble what we have seen in CX until
now, as much as possible. *pkg* is used to tell CX what package we
want to target, *fn* to target a function, and *exp* to target an
expression. In this case, *main*, *double* and *multiplication* are
not CX variables; they are simply identifiers for the inference
engine.

Now, we need to create something similar to a *knowledge base*,
containing *facts* or *objects*, as are called in CX. Objects simply
tell CX something that is true or, more correctly, something that
exists in the current environment.

```
objects := ->{
    cloudy $0.7,
    hot $0.2
}
```

As you can see, we keep using the *->{...}* syntax. We are stating
that the objects *cloudy* and *hot* exist. Notice that objects are
separated by commas, and that they have numbers next to them, preceded
by a dollar sign ($). These numbers are called *weights*, and they
help us assign a grade of truthiness to the object. For instance, we
are perceiving the weather to be 0.7 cloudy, or *very* cloudy perhaps,
while we are perceiving it to be only 0.2 hot, or *not so* hot.

Lastly, we need to define a set of *rules* that describe how the
stated objects are going to filter the affordances determined by the
affordance system.

```
if cloudy $0.8 {
  allow(x.lightSensitive == true)
  obj(drones $1.0)
}
if and(cloudy $0.5, hot $0.1) {
  allow(x.numberWheels > 2)
  reject(x.solarPowered == true)
  obj(rovers $1.0)
}
if true {
  allow(x.class == "bipedal")
}  
if or(drones $1.0, rovers $1.0) {
  reject(x.class == "bipedal")
}
```

Let's imagine that we want to program some logic that determines what
kind of robots can or should be deployed to a particular
environment. We need to keep in mind that
at the beginning, the affordance system should have thrown *every*
robot that exists in the system and that is accessible to the targeted
CX expression in this case.

The first rule is telling the inference system to allow any robot that
is light sensitive, and then we dynamically add another object to the
object set: *drones $1.0* (actually, this is an actual set, as in the
mathematical sense, i.e., objects can't be repeated). Rules can add
new objects to affect the execution of the following rules. Adding the
*drones* object can be interpreted as saying "we can/will deploy
drones."

The second rule tells the inference system to allow any robot that has
more than 2 wheels, but to reject any robot that is solar powered
(because it's too cloudy). The robots meeting these criteria are
rovers, so we add an object *rovers*.

The third rule is interesting because, regardless of what objects we
have and regardless of their weights, bipedal robots are going to be
deployed... *unless* the fourth rule is true. The fourth rule tells
the inference engine to reject bipedal robots if we are deploying
either drones or rovers already.

Now we have all the requirements to create a query: a target, objects
and rules. But first, we need to assign a label to the expression that
we want to query.

```
multiplication:
	i32.mul(5, 0)
```

The expression above is a simple multiplication. Notice how we are
multiplying 5 by 0. CX's affordance system, when querying expressions,
is always going find out what arguments we can send as the last output
that we have given. In this case, we want to replace the 0. To label
the expression, we use the same kind of labels that we use for *go-to*
statements.

```
affs := aff.query(target, objs, rules)
```

The code above will query the affordance system and perform the
filtering according to the provided objects and rules. Notice that
*aff.query* returns something and is stored in *affs*. *aff.query*
returns all the affordances for the queried expression. We can print
the obtained affordances by using *aff.print*:

```
aff.print(affs)
```

And should return something similar to:

```
(0)	Operator: AddArgument	Name: foo1
(1)	Operator: AddArgument	Name: foo2
(2)	Operator: AddArgument	Name: foo3
(3)	Operator: AddArgument	Name: foo4
```

The numbers enclosed in parentheses are the affordance indexes, and
they are used to know what affordance we want to execute or apply in
particular. For example:

```
aff.execute(target, affs, 0)
```

*aff.execute* executes the nth affordance from a list of affordances
 to a target. In this case, we want to apply the 0th affordance from
 *affs* to *target* (which is the multiplication
 expression). Following the robots example, we actually want to send
 all the robots that meet the criteria defined by the rules. We can
 loop over all the affordances by using *aff.len*:

```
for c := 0; i32.lt(c, aff.len(affs)); c = i32.add(c, 1) {
	aff.execute(target, affs, c)
deployRobot:
	deployRobot(new Robot{class: "default"})
}
```

You should always send a default value, in case the affordance system
returns 0 results. Also, CX will complain that *deployRobot* requires
one argument and won't compile or interpret the program. If you don't
want a default value, just send a dummy value like 0, an empty
struct instance, etc., and then have an *if* statement check if the
resulting affordances array is empty before continuing with *aff.execute*.

# Experimental Features

CX continues growing every day. As a consequence, some features are
still in its infancy stages. This also means that they are very prone
to change and be improved in the future. Particularly, in this
tutorial we'll mention CX's evolutionary algorithm, CX's capability to
serialize itself, and its OpenGL API.

## Evolutionary Algorithm

After creating the first prototype of CX's affordance system, one of
the first ideas that came to mind was to use it to create an
evolutionary algorithm; particularly, a genetic programming (GP)
algorithm. Programming languages that can manipulate themselves or
that can programmatically create programs are usual targets to
experiment with new GP ideas, such as lisps.

In the case of CX, the affordance system is ideal for creating such
algorithms. CX can start with a null object that represents a CX
program, and then query that object to know what we can do with it. As
it is empty, the first and only action would be to add a *main*
package. The second option only option would be to create a *main*
function. From this point, the possibilities start to grow
exponentially: add new definitions, new functions, new input and
output parameters, add expressions to these functions, etc. Actually,
one of the first experiments using the affordance system was to create
a random program.

With some restrictions, it was easy to create a basic GP algorithm for
CX: we need to target a single function which will act as a solution
to a problem, this function needs to have only one input and one
output (although this will probably change later on), we'll be adding
only a limited number of expressions to the function, the operators
for these expressions are limited to a certain set and, lastly, the
arguments to these expressions are limited to local variables only. We
could provide a deeper explanation on how it is constructed, but
that's out of the scope of this tutorial, but you can [read the
source code](https://github.com/skycoin/cx/blob/master/src/base/evolution.go).

Let's suppose that we have an function that we don't know how it
works, but we know what are its outputs when sent certain inputs (for
example, a stock market time-series).

```
[-10 -9 -8 -7 -6 -5 -4 -3 -2 -1 0 1 2 3 4 5 6 7 8 9 10]
```

We decide to test the function with the input values above, and the
unknown function responded with the following outputs:

```
[110 90 72 56 42 30 20 12 6 2 0 0 2 6 12 20 30 42 56 72 90]
```

How can we know what function responds that way? Well, one option is
to just think a little but and after some minutes you should find out
that a possible function is *n\*n+n*, but let's pretend that this is a
harder problem. How can we find a solution to this problem? One way is
to use a [curve-fitting](https://en.wikipedia.org/wiki/Curve_fitting)
algorithm, such as neural networks or, you guessed it, genetic
programming.

First, let's program the *real function*:

```
func realFn (n f64) (out f64) {
	out = f64.add(f64.mul(n, n), n)
}
```

And now, let's create the function that *simulates* the real function:

```
func simFn (n f64) (out f64) {}
```

Woa! Wait, it's empty! Yeah, we're going to ask CX to fill it for us
using its evolutionary algorithm. If we had a rough idea of how the
real function is composed, we could help CX by writing some
expressions that approximate the solution, like:

```
func simFn (n f64) (out f64) {
  someHelp := f64.mul(n, n)
}
```

And the evolutionary algorithm could converge to a solution faster (or
maybe not. Maybe your guess is not as good as you think). But let's
make CX do all the work and leave *simFn* empty. Now, we only need to
call *evolve*:

```
evolve("simFn", "f64.add|f64.mul|f64.sub", inps, outs, 5, 100, f32.f64(0.1))
```

*evolve*'s first parameter is used to indicate CX what's the target
 function to be evolved, which is *simFn* in this case. The second
 parameter is a string representing a bag of functions to be used to
 find the solution. If you have no idea of what could be part of the
 solution, you could just write "." and CX will use every function it
 knows to create a solution. The third and fourth parameters are the
 inputs and outputs that represent the real function's behaviour. The
 sixth parameter represents how many expressions you want the solution
 to have.

The last two parameters are known as *stop criteria*, which
 are: for how many iterations do you want CX to run the evolutionary
 algorithm, and what is a "good enough" error to reach before
 stopping. But why do we need these parameters? Why not run the
 evolutionary algorithm until it finds *THE* solution? Well, for this
 example we have chosen a very easy problem to solve, but most
 problems in the real world are very hard to solve. *Bio-inspired*
 search algorithms, such as GP algorithms, are considered as
 *heuristics*, which is a fancy word to say that they won't
 necessarily reach the optimal solution. There are indeed algorithms
 that are guaranteed to find an optimal solution, but these algorithms
 can take a lot of time to find it (and by a lot of time, we mean
 weeks or even months, depending on the problem and how rich you are
 te get a suitable server, of course). Anyway, *stop criteria* are
 used to tell the evolutionary algorithm when it should stop
 searching.

Anyway, after either reaching 100 iterations or an error lower or
equal to 0.1, our evolutionary algorithm will stop, and now we can
test the solution:

```
str.print("Testing evolved solution")
for c := 0; i32.lt(c, []f64.len(inps)); c = i32.add(c, 1) {
	f64.print(simFn([]f64.read(inps, c)))
}
```

As the problem is very easy to solve, the code above should print the
same numbers that are present in the outputs array. If you are curious
on how the evolved function looks like, you can add a call to *halt*
before the program finishes, and write *:dProgram;* in the *REPL*. You
should see something like this:

```
1.- Function: simFn (n f64) (out f64)
			0.- Expression: var_1037 = f64.mul(n f64, n f64)
			1.- Expression: var_9874 = f64.sub(var_1037 , var_1037 )
			2.- Expression: var_9905 = f64.mul(var_9874 , n f64)
			3.- Expression: var_9936 = f64.add(var_1037 , var_9874 )
			4.- Expression: out = f64.add(var_9936 , n f64)
```

That function is an equivalent function to *n\*n+n*. Awesome, right?

## Serialization

CX programs can fully or partially serialize themselves. At the
moment, CX's serialization feature is in a prototypical 

## OpenGL API

# Examples
## Fibonacci
## Factorial
## String Concatenatio
## Evolving a Function
## Serialization
## Robot Simulator
## Text-based Adventure Game
## Graphical Game
## Entity Component System (ECS) Engine
## Graphical Game using ECS


# CX Programming Language

This is a prototype version of the CX Programming Language. At the
moment, a programmer can either use the base language directly
(although this is no longer encouraged) or can use a programming
language with a syntax similar to Go. The reader can find a more
updated source of information at the [CX website](http://cx.skycoin.net/).

This README file described how to create a CX program using the base
language (by using Golang's functions and structs). These sections
will now appear at the end of this file (after the "Old README" title).

## Building the CX Compiler

### Quick Instructions

Run

```
$ chmod +x cx.sh
$ ./cx.sh
```

in a terminal. This script should install the necessary dependencies
and compile the CX compiler/interpreter.

To start a CX REPL, run

```
$ chmod +x cx
$ ./cx
```

If successful, one should see printed the following message:

```
CX REPL
More information about CX is available at https://github.com/skycoin/cx

*
```

To test the environment, write `:dProgram true;` to enable program
structure debugging.

After running the `cx.sh` script, one can now use the generated `cx` compiler
to run the REPL or compile source code files (just make sure to make it
executable by running `chmod +x cx`).

## CX REPL

CX now has a REPL where a programmer can interactively build and
evaluate a CX program. By following the instructions in the *Quick
Instructions* section, you should now have be in the CX REPL.

An important note is that a semicolon (;) is necessary at the end of
the input to indicate the REPL that you want to evaluate the code.

### Metaprogramming Commands

CX has a set of metaprogramming commands, which give the programmer
 more control over the building process of a program.

#### Debugging

CX has two metaprogramming commands for debugging: `:dProgram` and
`:dStack`. The former prints the program abstract syntax tree (AST)
whenever a new element is added (e.g., a new module, a new function, a
new expression), and the latter prints the callstack everytime a
forward step is performed (see the subsection *Stepping*).

To activate program structure debugging, write in the REPL:

```
:dProgram true;
```

To deactivate it, write:

```
:dProgram false;
```

`:dStack` helps the programmer know what are the inputs and outputs of
the functions being called during a program's execution, as well as
what expression is currently being evaluated. To activate it, write:

```
:dStack true;
```

And to deactivate it, write:

```
:dStack false;
```

#### Call Stack Stepping

The `:step` metaprogramming command receives an integer which
represents how many steps in the CX call stack should be run (or
"unrun"). `:step 10` would run the current program for 10 steps, while
`:step -5` would go back in time 5 steps.

For example, we could emulate a loop which repeats 3 times a block of code
with 5 expressions 3 times, by running:

```
:step 5 :step -5 :step 5 :step -5 :step 5 :step -5;
```

This is not an actualy loop, as it would not be updating the program's
state at each iteration. For example, if we have this expression `i =
addI32(i, 1)` and we run `:step 1 :step -1 :step 1 :step -1`, the
value of i stays the same at this point.

#### Selectors

A problem that many REPLs have is that they don't provide any feature
to modify an already defined element in a program (you usually need to
redefine the whole elemnt). In CX, you can *select* an element, like a
function, and continue appending expressions to it without having to
redefine the whole function.

For example, after running `./cx.sh` or `./cx`, we can start adding
expressions to the already defined and empty `main` function by
running:

```
:func main;
```

This command will print `:func main {...` indicating us that we are
now under the scope of the `main` function. Any expression that we
introduce now will be appended to `main`. To exit from this scope, we
can just press `Ctrl + D`.

If we only want to append a few expressions and then immediately
return to the previous scope, we can run:

```
:func main {addI32(3, 10) /* ... more expressions ... */};
```

All the available selectors are:

* `:package IDENT;`, `:package IDENT {LINES};` to switch to another
package or append lines to another package
* `:func IDENT`, `:func IDENT {EXPRESSIONS}}` to switch to another
function or append expressions and statements to another function
* `:struct IDENT`, `:struct IDENT {FIELDS}` to switch to another
struct or append fields to another struct

Soon, the CX REPL should also have *structure stepping*, which will
allow us to remove the previous N elements in a CX program. Structure
stepping, combined with selectors will allow a more powerful REPL.

#### Affordances

At the moment, the CX REPL only provides access to function
affordances. The `:aff` metaprogramming command has three different
behaviours: 1) it can print the available affordances for a desired
function; 2) it can filter the affordances using a regular expression
; and 3) it can apply an affordance.

To list all the available affordances, you can run `:aff func IDENT;`,
for example:

```
:aff func main;
```

This will print a list of the affordances to the terminal. The list
looks something like this:

```
...
(27)	AddOutput i32
(28)	AddOutput i64
(29)	AddOutput str
(30)	AddExpression core.addF32 (f32, f32) (f32)
(31)	AddExpression core.addF64 (f64, f64) (f64)
(32)	AddExpression core.addI32 (i32, i32) (i32)
...
```

We can then apply, for example, affordance #28 by running `:aff func
main {28};`.

If we want to only list the affordances related to adding input
parameters to a function, we can run `:aff func main {"input"}`. Then
we can select one of the available affordances with `:aff func main
{"input" 3}`.

#### Loading a file to the REPL

We can load an existing source file by passing the `--load` flag to
the REPL. The *golike/* directory contains a *test.cx* file that can
be loaded:

```
# this (re)compiles everything and then loads the file to the REPL
./cx.sh --load test.cx
```

or

```
# this uses the generated CX executable to load the file to the REPL
./cx --load test.cx
```

To test if the file was loaded, we can print the program's AST by
running `:dProgram true;`. If we input `:step 2;` to the REPL, we
should see:

```
fn:main ln:0, 	locals: 
10
>> 10
fn:main ln:1, 	locals: 
20
>> 20
```

The first two expressions in the *test.cx* file contain two calls to
`printI32`. The REPL is printing 10 and 20, and at the same time it's
telling us that they are returning these values. This means that the
following expression is valid:

```
addI32(printI32(10), printI32(20))
```

and it will first print 10, then 20, and finally 30.

### Interpreting a Source File

We can avoid entering the REPL by just passing a source file to the CX
interpreter as the first argument in the terminal:

```
./cx test.cx
```

This should only print:

```
10
20
12
```

## Go-to Native Function

The flow control statements in CX are not performed by Go's native
statements.  A go-to native function had to be implemented to
enable flow control statements like if, if/else and while loops.

Basically, goTo increments or decrements the `Line` number of an
expression in a `CXCall` structure. The `goTo` function receives three
arguments: a predicate, an increment or decrement of lines in case the
predicate evaluates to true, and an increment or decrement of lines in
case the predicate evaluates to false.

Considering the following CX function:

```
func basicIf (num i32) (num i32) {
  pred := gtI32(num, 0)
  goTo(pred, 3, 4)
  printStr("Greater than 0")
  printStr("Less than 0")
}
```

if we call `basicIf(-1)`, the program should print "Less than 0",
ignoring the `printStr("Greater than 0")` expression. As we can see in
the goTo expression, if predicate is true, the program will jump
to line #3, and if the predicate is false, the program will
jump to line #4.

If we call `basicIf(3)`, the program will print "Greater than 0" and
"Less than 0". The reason behind this is that the program is jumping
to line #3, but we're not telling it to ignore line #4, i.e., the
program will continue a normal flow.

If we wanted to implement a more correct if/else structure, we'd need
to add another goTo expression after the `printStr("Greater than 0")`
expression, telling the program to jump to line #5 (which doesn't
exist, but CX interprets any line number greater than the number of
expressions in a function as the end of the function's execution).

## Go-like Language

Nex (a Lex-like lexical analyzer) and Go's Yacc were used to create a
parser for a go-like language which compiles to CX base. The syntax is
almost identical to Go's.

### Packages

Packages can be defined by using the keyword `package` followed by an
identifier. Every CX program needs a main module, so let's define one:

```
package main
```

### Global Variables

Global variables can be created by using the `var` keyword. The type
must always be provided (there's no type inference yet), and they must
always be initialized for now.

Let's test the evolutionary algorithm to demonstate the current
capabilities of the go-like programming language. We already have the
*main* package defined, now we need some global variables defining the
inputs and the outputs for the supervised training of the solution
model:

```
var inps []i32 = []i32{
	-10, -9, -8, -7, -6, -5, -4, -3, -2, -1,
	0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    
var outs []i32 = []i32{
	-970, -702, -488, -322, -198, -110, -52, -18, -2,
	2, 0, -2, 2, 18, 52, 110, 198, 322, 488, 702, 970}
```

For illustrative purposes, let's also define a some more variables:

```
var greeting str = "hello"
var foo i32 = 10
var bar i32 = 20
var epsilon f64 = 1.531
```

### Functions

For debugging and testing reasons, for now we are required to write
prototypes of the native functions we want to use in a package:

```
/* Identity functions
func idAI32 (x []i32) (x []i32) {}
func idI32 (x i32) (x i32) {}
*/
func addI32 (n1 i32, n2 i32) (out i32) {}
func subI32 (n1 i32, n2 i32) (out i32) {}
func mulI32 (n1 i32, n2 i32) (out i32) {}
// func readByteA (arr []byte, idx i32) (byt byte) {}
func evolve (fnName str, inps []i32, outs []i32, numberExprs i32, iterations i32, epsilon f64) (success i32) {}
```

The first two functions are identity functions (`f(x) = x`) for the
i32 and []i32 types. As they would be useless in the evolutionary
algorithm, let's comment them using a multiple-line comment (/*
... */). The readByteA function doesn't play well with the
evolutionary algorithm, so it's commented using a single line comment
(//).

The remaining native functions are *addI32*, which adds two i32
arguments, *subI32* which substracts two i32 arguments,
*mulI32* which multiplies two i32 arguments, and *evolve*
which takes the name of a function defined in the current package and
evolves it to fit an array of outputs. It currently outputs an i32
argument which tells us if the evolution was successful or not. This
should be changed later for a more successful output (maybe the error
of the best solution).

### Expressions

The functions above are expressionless functions. Let's now create a
function with an expression:

```
func double (num i32) (out i32) {
	out = addI32(num, num)
}
```

This function simply takes a number and doubles it using the *addI32*
native function. Notice how we had to use the `out` varible to assign
the result of the addition. As this is the same variable name as the
output parameter's name, the argument returned by this expression will
be the output of the `double` function. The assignment operator, for
now, can either be written as  `=` or `:=`.

### The Solution Function

Before defining the `main` function, we need to define a function to
be evolved by the evolutionary algorithm (although we could evolve any
function we wanted):

```
func solution (n i32) (out i32) {}
```

This function will take as input each of the integers in the `inps`
array, and must return the corresponding output integer from the
`outs` array.

Something interesting to note is that this function could have some
expressions in it. If we have an idea of an approximate solution to
the problem we want the evolutionary algorithm to solve, we can
transform the training stage to a semi supervised learning process
this way.

The solution we want to arrive to is:

```
func solution (n i32) (out i32) {
	double = addI32(n, n)
	triple = addI32(double, n)
	square = mulI32(n, n)
	cubic = mulI32(square, n)
	out = subI32(cubic, triple)
}
```

If we the function above, the evolutionary algorithm should stop at
iteration 0. If we comment out some of the expressions, but not all,
the algorithm should converge faster to the correct solution (at least
most of the time; it could also kill our expressions in the first
iterations via mutation).


### The Main Function

Just like the `main` module, every CX program needs a `main` function,
which acts as the entry point for the program. For now, any input
parameters we add to the main function will be ignored. In the case of
the output parameters, the first one will be used as the program's
final output.

```
func main () (outMain i32) {
	_ := evolve("solution", inps, outs, 20, 10000, epsilon)
	outMain := solution(30)
}
```

In the `main` function above, the first expression calls the `evolve`
function. We are telling `evolve` to perform the evolutionary process
on the `solution` function, using the `inps` and `outs` arrays for
training. We want the solution function to have exactly 20
expressions, the evolutionary algorithm will run for 10,000 iterations
and should stop if *epsilon* is reached (if the error is less than
*epsilon*).

Finally, we test the evolved solution by calling `solution(30)` and
output this to the program's output variable `outMain` (which is
`main`'s output paramater).

## CX 'If/else' Flow Control Structure

Let's suppose we want to decide between two different operations to be
assigned to variable `result`. In CX, we can create an if/else
structure in the following way:

```
    pred := gtI64(5, 10)
	if pred {
		result := addI64(7, 10)
		//print(25)
	} else {
		result := addI64(20, 3)
	}
```

Notice that we can't place the predicate inside the if/else
structure. This is a problem with the current implementation of the
parser.

Internally, the if/else structure above will be transformed to the
following series of expressions:

```
    1.- Expression: pred = gtI64(5 i64, 10 i64)
	2.- Expression: goTo(pred ident, 1 i32, 3 i32)
	3.- Expression: result = addI64(7 i64, 10 i64)
    4.- Expression: goTo([1] byte, 2 i32, 0 i32)
    5.- Expression: result = addI64(20 i64, 3 i64)
```

## CX 'while' Flow Control Structure

At the moment, CX only has implemented the while loop. An example of a
while loop which prints the numbers from 0 to 10,000 is:

```
    var i i64 = 0
	pred := ltI64(i, 10000)
	while pred {
		//printI64(i)
		i := addI64(i, 1)
		pred := ltI64(i, 10000)
	}
```

Internally, the code above will be transformed to the following series
of expressions:

```
    10.- Expression: i = idI64(0 i64)
    11.- Expression: pred = ltI64(i ident, 10000 i64)
    12.- Expression: goTo(pred ident, 1 i32, 5 i32)
    13.- Expression: printI64(i ident)
    14.- Expression: i = addI64(i ident, 1 i64)
    15.- Expression: pred = ltI64(i ident, 10000 i64)
    16.- Expression: goTo(pred ident, -3 i32, 1 i32)
```

We can notice how the final `goTo` receives a negative argument as its
second parameter. This makes the program go back 3 lines in order to
make another iteration. As soon as the predicate is false, the program
will advance 1 line (goTo's third parameter), as in normal execution.







# Old README

## Basic Usage

```
package main

import (
    "fmt"
    "github.com/skycoin/skycoin/src/cipher/encoder"
    . "github.com/skycoin/cx/src/base"
)

func main() {

}

```

Start by creating a reference to the only currently supported primitive
type *i32*.

```
    i32 := MakeType("i32")
```

We will use the variable *i32* when creating objects that require a
type, like *definitions* and *fields* in structures. When executing a
function which uses definitions of type i32, the base language will
automatically recognize this primitive type.

Arguments that represent identifiers need to be of type *ident*. These
identifiers will point to a value of certain type (like i32). This
behaviour is similar to how Lisp handles symbols, and how symbols
point to other data. When using languages constructed using the CX
base language, this process should be transparent to the user. For the
moment, let's create the type *ident*:

```
    ident := MakeType("ident")
```

The last reference to a primitive that we need to make is to the
*addI32* function:

```
    addI32 := MakeFunction("addI32")
```

which simply adds two arguments of type i32.

A function is created using a *maker*, which is, in this case,
*MakeFunction*:

```
    double := MakeFunction("double")
```

This function will take a number, will double it and then will return
the result. Let's start with adding the input and the output:

```
    double := MakeFunction("double").
        AddInput(MakeParameter("num", i32)).
        AddOutput(MakeParameter("", i32))
```

*Adders* are used to add objects to another object. In this case,
*AddInput* and *AddOutput* were used to add an input parameter and an
output parameter to the double function, respectively. Both of these
adders require a *parameter* object, which is created by using the maker
*MakeParameter*. The input parameter has the name "num" and is of type
i32. The output is unnamed and is of type i32. In the CX base
language, one can give the output a name or not. The output of a
function will correspond to the name of the output parameter in the former case,
or will be the value of the last expression in the function body in
the latter. As this function will only require a single expression, an
unnamed output is more convenient. Let's now add this expression:

```
    double := MakeFunction("double").
		  AddInput(MakeParameter("num", i32)).
		  AddOutput(MakeParameter("", i32)).
		
		  AddExpression(MakeExpression(addI32).
		                AddArgument(MakeArgument(MakeValue("num"), ident)).
	                    AddArgument(MakeArgument(MakeValue("num"), ident)))
```

The maker *MakeExpression* requires an operator to make an expression,
which is addI32 in this case. Without any arguments, this maker would
create this s-expression: `(+)`. After adding twice the *num*
identifier to the expression, the s-expression would look like this:
`(+ num num)`.

The double function could be represented by the following
s-expression:

```
(defun double (num i32)
    (+ num num))
```

If we want to call this function, and actually double a number with
it, we can make another expression which uses *double* as its
operator. But first, let's create a variable which holds the number
25:

```
num := encoder.SerializeAtomic(int32(25))
```

As values in CX are serialized to byte arrays, we used Skycoin's
`cipher/encoder` package to serialize the number. Now, to double this
number and print the value:

```
fmt.Println(
		MakeExpression(double).
			AddArgument(MakeArgument(&num, i32)).
			
			Execute(mod.Definitions).Value)
```

In the next versions, a program should be executed as this
`cxt.Execute()` where `cxt` is the context holding references to
everything in a program. The program would run by executing a `main()`
function or by continuing executing the function and expression
in which it was paused at. For now, we need to create an expression
which uses *double* and the number we created as its argument, and
then call its `Execute` method. The execute method requires a
definitions array to act as its state.

By compiling and running the code above, this output should be
printed:

```
&[50 0 0 0]
```

which is a seralized 50 int32.

## Affordances

An affordance of an object represents something that we can do to that
object. At the moment, CX can apply some basic affordances on its
objects, like *adders* and *selectors*. Adders were used in the
previous section, and these basically add objects to other objects
(like adding an input parameter to a function). Selectors change the
current object the program is acting on: for example,
`SelectModule` will change a context to work on a desired module,
which means that any call to `AddFunction`, `AddStruct` or
`AddDefinition` will add objects to this module.

Let's create a "root" object or context and call its `GetAffordances`
method:

```
    cxt = MakeContext()
	PrintAffordances(cxt.GetAffordances())
```

`PrintAffordances` is a function used for debugging affordances; it
just prints an affordance's description to the console. The code above
should give the following output:

```
AddModule mod26
```

This is telling us that the only affordance over our empty context is
to add a module. If we decide to apply this affordance, the program
will generate a unique, but not very descriptive, identifier (mod26 in
this case). Let's apply this affordance:

```
    cxt.GetAffordances()[0].ApplyAffordance()
```

Note that we had to state that we desire to apply affordance
`0`. Other functions should provide the logic to follow in order to
determine what affordance to apply. A `FilterAffordances` function is
provided, which receives a series of string keywords to filter the
descriptions of the provided affordances. For example:

```
FilterAffordances(cxt.GetAffordances(),
    "AddExpression", "write")
```

this filter will return all the affordances which involve adding an
expression with operators containing the "write" keyword.

After applying the affordance obtained above, if we make another call
to `GetAffordances`, we should get an output similar to this:

```
AddModule mod27
SelectModule mod26
```

It is now telling us that we can either add another module or select
the previously created module. Let's now run the following code:

```
    PrintAffordances(cxt.GetCurrentModule().GetAffordances())
```

We are using a *getter* called `GetCurrentModule` on our context to
obtain the active module, and then we get its affordances and print
them. The output should be similar to this:

```
AddDefinition def27 i32
AddFunction fn28
AddStruct strct29
```

This tells us that we can add a new definition of type i32, a function
or a struct to the module. If we had more types (structs and more
primitive types), the list of *AddDefinition*s would be longer (one
for each available type in the current module, and for each imported
module).

If we want to know the structure of a program (context), we can use
the function `PrintProgram`, which is a function that has been used
only for debugging purposes. By running `cxt.PrintProgram(false)`, we
should get something like this:

```
Context
0.- Module: mod0
1.- Module: Math
	Definitions
		0.- Definition: def2 i32
		1.- Definition: def3 Complex
		2.- Definition: hugs i32
		3.- Definition: items StdLib.Stream
		4.- Definition: views i64
	Structs
		0.- Struct: Complex
		1.- Struct: List
			0.- Field: fld18 i32
			1.- Field: fld23 List
	Functions
		0.- Function: fn5 (in16 StdLib.Stream, in17 StdLib.Stream, num1 i32, num2 i32) sum i32
		1.- Function: fn10 (items StdLib.Stream) 
			0.- Expression: fn5(items StdLib.Stream, items StdLib.Stream, hugs i32, hugs i32)
2.- Module: StdLib
	Structs
		0.- Struct: Stream
	Functions
		0.- Function: fn14 () 
```

The `false` argument tells the function to not print the available
affordances for each object. Here is an example of this case:

```
Context
 * 0.- AddModule mod26
 * 1.- SelectModule mod0
 * 2.- SelectModule Math
 * 3.- SelectModule StdLib
 * 4.- SelectFunction fn5
 * 5.- SelectFunction fn10
 * 6.- SelectStruct List
 * 7.- SelectStruct Complex
 * 8.- SelectExpression Line # 0
0.- Module: mod0
	 * 0.- AddDefinition def27 i32
	 * 1.- AddImport Math
	 * 2.- AddImport StdLib
	 * 3.- AddFunction fn28
	 * 4.- AddStruct strct29
1.- Module: Math
	 * 0.- AddDefinition def30 i32
	 * 1.- AddDefinition def31 List
	 * 2.- AddDefinition def32 Complex
	 * 3.- AddDefinition def33 StdLib.Stream
	 * 4.- AddImport mod0
	 * 5.- AddImport StdLib
	 * 6.- AddFunction fn34
	 * 7.- AddStruct strct35
	Definitions
		0.- Definition: def2 i32
		1.- Definition: def3 List
		2.- Definition: hugs i32
		3.- Definition: items StdLib.Stream
		4.- Definition: views i64
	Structs
		0.- Struct: List
		 * 0.- AddField fld36 i32
		 * 1.- AddField fld37 List
		 * 2.- AddField fld38 Complex
		 * 3.- AddField fld39 StdLib.Stream
			0.- Field: fld18 i32
			1.- Field: fld23 List
		1.- Struct: Complex
		 * 0.- AddField fld40 i32
		 * 1.- AddField fld41 List
		 * 2.- AddField fld42 Complex
		 * 3.- AddField fld43 StdLib.Stream
	Functions
		0.- Function: fn5 (in16 StdLib.Stream, in17 StdLib.Stream, num1 i32, num2 i32) sum i32
		 * 0.- AddInput i32
		 * 1.- AddInput List
		 * 2.- AddInput Complex
		 * 3.- AddInput StdLib.Stream
		 * 4.- AddExpression fn10
		 * 5.- AddExpression StdLib.fn14
		1.- Function: fn10 (items StdLib.Stream) 
		 * 0.- AddInput i32
		 * 1.- AddInput List
		 * 2.- AddInput Complex
		 * 3.- AddInput StdLib.Stream
		 * 4.- AddOutput i32
		 * 5.- AddOutput List
		 * 6.- AddOutput Complex
		 * 7.- AddOutput StdLib.Stream
		 * 8.- AddExpression fn5
		 * 9.- AddExpression StdLib.fn14
			0.- Expression: fn5(items StdLib.Stream, items StdLib.Stream, def2 i32, hugs i32)
2.- Module: StdLib
	 * 0.- AddDefinition def44 i32
	 * 1.- AddDefinition def45 Stream
	 * 2.- AddImport mod0
	 * 3.- AddImport Math
	 * 4.- AddFunction fn46
	 * 5.- AddStruct strct47
	Structs
		0.- Struct: Stream
		 * 0.- AddField fld48 i32
		 * 1.- AddField fld49 Stream
	Functions
		0.- Function: fn14 () 
		 * 0.- AddInput i32
		 * 1.- AddInput Stream
		 * 2.- AddOutput i32
		 * 3.- AddOutput Stream
```

## Randomly Generating a Program

A function which continually applies affordances to an empty program
is provided. This function, `RandomProgram`, receives a number that
represents how many affordances will be applied, and returns the
generated context. For example, by running

```
    cxt := RandomProgram(100)
    cxt.PrintProgram(false)
```

we get an output similar to this:

```
0.- Module: mod26
	Definitions
		0.- Definition: def27 i32
		1.- Definition: def33 i32
		2.- Definition: def39 i32
	Functions
		0.- Function: fn31 (in36 i32, in38 i32, in51 i32) 
1.- Module: mod42
	Definitions
		0.- Definition: def113 mod42.strct89
		1.- Definition: def119 strct89
		2.- Definition: def135 mod42.strct89
		3.- Definition: def142 mod42.strct48
		4.- Definition: def63 strct48
		5.- Definition: def75 i32
	Structs
		0.- Struct: strct48
			0.- Field: fld54 strct48
			1.- Field: fld57 strct48
			2.- Field: fld59 i32
		1.- Struct: strct89
			0.- Field: fld94 mod42.strct89
			1.- Field: fld99 mod42.strct89
			2.- Field: fld128 strct89
	Functions
		0.- Function: fn44 (in49 strct48, in55 strct48, in58 strct48, in61 strct48, in66 strct48, in67 strct48, in68 strct48, in69 strct48, in95 mod42.strct48) 
		1.- Function: fn106 (in108 mod42.strct48, in116 mod42.strct89, in117 mod42.strct89, in138 mod42.strct89) 
2.- Module: mod146
```

which sadly didn't add any expression to any function it created (so
it wouldn't do anything if executed).

## Evolutionary Algorithm

For this small evolutionary algorithm, affordances are used to add
expressions to a function which represents a solution to a curve
fitting problem. Mutation is defined by randomly removing one expression from
a program, and then applying an affordance to replace the removed
expression. At the moment, all the necessary code to perform the
evolutionary process is located in the *base/evolution.go* file.

For this walkthrough, the observed points are obtained by evaluating
`f(x) = x*x*x - (3*x)` with the integer set `{-10, -9, 8, ..., 8, 9,
10}`. In *main.go*, these points are obtained by the following code:

```
dataIn := []int32{-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

	dataOut := make([]int32, len(dataIn))
	for i, in := range dataIn {
		dataOut[i] = in * in * in - (3 * in)
	}
```

The context (or program) must have at least a module named `main`, a
function named `main`, a solution function named `solution`, and the
`main` function must call solution with the input data as
arguments. Also, one can add the desired functions to be used as
operators for the expressions in the solution function (it can be
noted that more functions means a larger search space for the
evolutionary algorithm). For this walkthrough, the initial program is
defined by the following code:

```
num1 := encoder.SerializeAtomic(int32(0))

	cxt := MakeContext().AddModule(MakeModule("main"))
	if mod, err := cxt.GetCurrentModule(); err == nil {
		mod.AddDefinition(MakeDefinition("num1", &num1, i32))

		mod.AddFunction(MakeFunction("addI32"))
		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddInput(MakeParameter("n1", i32))
			fn.AddInput(MakeParameter("n2", i32))
			fn.AddOutput(MakeParameter("out", i32))
		}
		
		mod.AddFunction(MakeFunction("subI32"))
		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddInput(MakeParameter("n1", i32))
			fn.AddInput(MakeParameter("n2", i32))
			fn.AddOutput(MakeParameter("out", i32))
		}

		mod.AddFunction(MakeFunction("mulI32"))
		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddInput(MakeParameter("n1", i32))
			fn.AddInput(MakeParameter("n2", i32))
			fn.AddOutput(MakeParameter("out", i32))
		}
		
		mod.AddFunction(MakeFunction("main"))
		mod.AddFunction(MakeFunction("double"))

		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddInput(MakeParameter("n", i32))
			fn.AddOutput(MakeParameter("out", i32))
			if addI32, err := cxt.GetFunction("mulI32", mod.Name); err == nil {
				fn.AddExpression(MakeExpression("out", addI32))

				if expr, err := cxt.GetCurrentExpression(); err == nil {
					expr.AddArgument(MakeArgument(MakeValue("n"), ident))
					expr.AddArgument(MakeArgument(MakeValue("n"), ident))
				}
			}
		}

		mod.AddFunction(MakeFunction("solution"))

		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddInput(MakeParameter("x", i32))
			fn.AddOutput(MakeParameter("f(x)", i32))
		}
		
		mod.SelectFunction("main")

		if fn, err := cxt.GetCurrentFunction(); err == nil {
			fn.AddOutput(MakeParameter("outMain", i32))
			if solution, err := cxt.GetFunction("solution", mod.Name); err == nil {
				fn.AddExpression(MakeExpression("outMain", solution))
				
				if expr, err := cxt.GetCurrentExpression(); err == nil {
					expr.AddArgument(MakeArgument(MakeValue("num2"), ident))
				}
			}
		}
	}
```

Each of the inputs is going to be assigned to the global definition
`num1` (the reason for this is the mere convenience of reusing the
code for previous testings). As can be seen in the code, the operators
which can be used for the expressions are `addI32`, `subI32`,
`mulI32`, and `double`.

The `solution` function receives a single parameter `x`, and has the
named output `f(x)`.

The evolutionary process can be started by calling the method
`EvolveSolution` on the previously defined context:

```
    evolvedProgram := cxt.EvolveSolution(dataIn, dataOut, 5, 10000)
```

`EvolveSolution` takes four parameters: the data inputs, the data
outputs, the number of expressions which the solution should have, and
the number of maximum iterations the algorithm can perform, in that
order. In this case, the solution should have exactly five expressions
and the maximum number of iterations is 10,000. This exercise was ran
several times, and the optimal solution was always found in less than
1,000 iterations, so a stop condition was added to the algorithm,
which prevents any more evaluations to occur if the error is equal
to 0. An additional parameter (usually called epsilon) can be added to indicate the
algorithm to stop if the error is lower than this threshold.

While the algorithm is running, one can see the lowest achieved error
at each iteration printed to console. Once the evolutiona process has
finished, the evolved program can be printed by running:

```
    evolvedProgram.PrintProgram(false)
```

which should print something like this:

```
Context
0.- Module: main
	Definitions
		0.- Definition: num1 i32
		1.- Definition: num2 i32
	Functions
		0.- Function: solution (x i32) f(x) i32
			0.- Expression: x = mulI32(num1 i32, num1 i32)
			1.- Expression: var7388 = mulI32(x i32, num1 i32)
			2.- Expression: x = subI32(var7388 ident, num1 i32)
			3.- Expression: var38465 = addI32(num1 i32, num1 i32)
			4.- Expression: f(x) = subI32(x i32, var38465 ident)
		1.- Function: addI32 (n1 i32, n2 i32) out i32
		2.- Function: subI32 (n1 i32, n2 i32) out i32
		3.- Function: mulI32 (n1 i32, n2 i32) out i32
		4.- Function: main () outMain i32
			0.- Expression: outMain = solution(num2 i32)
		5.- Function: double (n i32) out i32
			0.- Expression: out = mulI32(n i32, n i32)
```

The simulated vs observed points can be printed by running the
following code:

```
var error int32 = 0
	for i, inp := range dataIn {
		num1 := encoder.SerializeAtomic(inp)
		if def, err := evolvedProgram.GetDefinition("num1"); err == nil {
			def.Value = &num1
		} else {
			fmt.Println(err)
		}

		evolvedProgram.Reset()
		evolvedProgram.Run(false, -1)

		// getting the simulated output
		var result int32
		output := evolvedProgram.CallStack[0].State["outMain"].Value
		encoder.DeserializeAtomic(*output, &result)

		diff := result - dataOut[i]
		fmt.Printf("Simulated #%d: %d\n", i, result)
		fmt.Printf("Observed #%d: %d\n", i, dataOut[i])
		if diff >= 0 {
			error += diff
		} else {
			error += diff * -1
		}
	}
```

Which will print something similar to this:

```
Simulated #0: -970
Observed #0: -970
Simulated #1: -702
Observed #1: -702
Simulated #2: -488
Observed #2: -488
Simulated #3: -322
Observed #3: -322
Simulated #4: -198
Observed #4: -198
Simulated #5: -110
Observed #5: -110
Simulated #6: -52
Observed #6: -52
Simulated #7: -18
Observed #7: -18
Simulated #8: -2
Observed #8: -2
Simulated #9: 2
Observed #9: 2
Simulated #10: 0
Observed #10: 0
Simulated #11: -2
Observed #11: -2
Simulated #12: 2
Observed #12: 2
Simulated #13: 18
Observed #13: 18
Simulated #14: 52
Observed #14: 52
Simulated #15: 110
Observed #15: 110
Simulated #16: 198
Observed #16: 198
Simulated #17: 322
Observed #17: 322
Simulated #18: 488
Observed #18: 488
Simulated #19: 702
Observed #19: 702
Simulated #20: 970
Observed #20: 970
```

And finally, the obtained error can be calculated by dividing the
accumulated absolute errors by the number of data points:

```
fmt.Println(error / int32(len(dataIn)))
```

## Call Stack Stepping

A call to an expression is represented by a struct. This struct
stores a reference to the expression's operator which is being called,
the line number currently being executed in the expression's operator,
a state (a set of definitions), and a return address (to what call do we
need to return once the current call finishes its execution). A
context or a program stores an array of calls, which is defined as a
*call stack*.

Implementing the execution of a program (by a series of calls,
represented by structs) in this way allows a program to store the
call stacks a program is creating during its execution. The storing of
a call stack is defined as a *step*, and the process of storing call
stacks is defined as *stepping* (these names are subject to change and
I will use them for convenience for now).

Stepping in the current implementation happens everytime a new call is
performed. This means that a program has access to every past point of
execution. This behaviour can easily be changed to saving the program
steps every *N* calls, in order to save system resources.

`MakeContextCopy(cxt *cxContext, stepNumber int)` can be used to
create a copy of a program. The second parameter, *stepNumber*, is
used to indicate at which point of execution one wants to create the
copy. For example, if a program was executing a loop, we can create a
copy of this program and "rollback" the steps to the point where the
program hasn't entered the loop yet. The following code makes a copy
of the evolved program from the last section, rollbacks to step 3, and
then executes again:

```
    copy := MakeContextCopy(evolvedProgram, 8)
	copy.Run(true, -1)
```

The `Run` method above receives two parameters: `withDebug bool`,
which prints the call stack at each step of the program execution; and
`nCalls int`, which tells the program how many calls it must run
before pausing its execution. The program doesn't raises an error if
it finishes its execution before the *nCalls* threshold is reached. If
we don't want a program to run for a certain number of calls, we can
simply give it a negative number of calls.

The method `ResetTo(stepNumber int)` can be called on a program to
rollback to the given step number, without making any copies of the
program. The code below rollsback a program to step 3, and then runs
the same program only for 8 more steps. This process is looped 5 times.

```
    for i := 0; i < 5; i++ {
      evolvedProgram.ResetTo(3)
      evolvedProgram.Run(true, 8)
    }
```

It is important to note that each execution in the loop above is
independent from the others, state-wise, i. e., they won't share
the values of their variables among them.

Call stack stepping can be used later on for debugging a program while
it's being executed. For example, if we have an evolutionary that has
been running for the last 5 days and it encounters an error, we can
inspect the state in each of the calls in the current stack, encounter
the problem, rollback *N* steps, make the necessary changes to the
program structure, and then resume the execution.

Another use for call stack stepping is for benchmarking code blocks of a
running program. We can create two copies of a program, each using a
particular solution to a problem (for example, one that uses a
solution based on looping, and the other uses recursion). We set each
of these copies to the start of the desired step, and run each program
until it finishes the code block being benchmarked. The code block
with the better performance can then be inserted to the original
program.

## Program Structure Stepping

A program is also aware of its own structure and the ordered steps
that have to be executed to reach its current structure. This means
that a program has the capability of rolling back to a certain step in
its structure, and we can create copies of a program at different
points of its structure stepping.

With the current implementation of CX, we must create a new program
and use the program steps of another to duplicate its structure to the
new program. For example, if we have a program stored in the variable
`cxt`, we can create three partial copies of it in the following way:

```
    copy1 := MakeContext()
	copy2 := MakeContext()
	copy3 := MakeContext()

    for i := 0; i < 15; i++ {
		cxt.ProgramSteps[i].Action(copy1)
	}

	for i := 0; i < 5; i++ {
		cxt.ProgramSteps[i].Action(copy2)
	}
	
	for i := 0; i < 10; i++ {
		cxt.ProgramSteps[i].Action(copy3)
	}

    copy1.PrintProgram(false)
	copy2.PrintProgram(false)
	copy3.PrintProgram(false)
```

The first loop will create a copy of the program until step 15, the
second loop a copy until step 5, and the final loop a copy until step 10.

The code above will print something similar to this:

```
Context
0.- Module: main
	Definitions
		0.- Definition: num1 i32
		1.- Definition: num2 i32
	Functions
		0.- Function: addI32 (n1 i32, n2 i32) out i32
		1.- Function: subI32 (n1 i32, n2 i32) out i32
		2.- Function: mulI32 (n1 i32, n2 i32) out i32
Context
0.- Module: main
	Definitions
		0.- Definition: num2 i32
		1.- Definition: num1 i32
	Functions
		0.- Function: addI32 (n1 i32) 
Context
0.- Module: main
	Definitions
		0.- Definition: num1 i32
		1.- Definition: num2 i32
	Functions
		0.- Function: addI32 (n1 i32, n2 i32) out i32
		1.- Function: subI32 (n1 i32, n2 i32) 
```

With call stack stepping, program structure stepping, and evolutionary
algorithms we could create a program which stops itself at a
certain step, measures the execution time of one of its functions,
mutates them and if it creates something better, automatically
modify itself, and then resume its execution. The same could be done
to replace buggy parts of a program: if a function raises an exception with a
certain combination of arguments, we can mutate the function until it
gives us the same outputs as the previous version of the function, but
also doesn't raise the exception.

## Byte Arrays

The `byte` type has been added, as well as the `[]byte` type, which
defines a byte array. Two native functions to manipulate byte
arrays were added (found at *base/functions.go*): `readByteA` and
`writeByteA`.

`readByteA` takes two parameters: a byte array and an index. The index
(int32) is used to retrieve the byte at that position in the byte
array. The function returns a `byte`.

`writeByteA` takes three parameters: a byte array, an index and a byte
value. The byte at the indicated index in the byte array is replaced
by the provided byte value. The function returns the modified byte array.

## Serialization

A program can be serialized using the function `Serialize`. The result
of applying this method is a byte array which can be deserialized
using the function `Deserialize`.

Basically, the serialization process involves using a set of structs
defined in *base/serialize.go* which store offsets and sizes. An
*offset* field tells us at what index in the byte array we can find
the starting byte of another structure, and a *size* field tells us how
many bytes have to be sequentially read from the byte array to
retrieve a subset of bytes that represent another structure.

For example, the struct

```
type sModule struct {
	NameOffset int32
	NameSize int32
	ImportsOffset int32
	ImportsSize int32
	FunctionsOffset int32
	FunctionsSize int32
	StructsOffset int32
	StructsSize int32
	DefinitionsOffset int32
	DefinitionsSize int32
	CurrentFunctionOffset int32
	CurrentStructOffset int32
}
```

provides offsets and sizes of a module to find its module's name,
imported modules, functions, structs, and definitions. By using
`FunctionsOffset` and `FunctionsSize`, we can extract a subset of
bytes representing all the functions of that module. As every of these
"serialization structs" are fixed sized (they only contain int32
fields), we can use `encoder.Size()` to determine its serialized size,
and use it to loop over the serialized functions in the byte array. A
deserialized function looks like this:

```
type sFunction struct {
	NameOffset int32
	NameSize int32
	InputsOffset int32
	InputsSize int32
	OutputOffset int32
	ExpressionsOffset int32
	ExpressionsSize int32
	CurrentExpressionOffset int32
	ModuleOffset int32
}
```

Again, the offsets and sizes would be used to retrieve the elements
required to get back the desired function.

A program can be executed for *N* steps:

```
    cxt.Run(false, 2)
```

Then it can be serialized:

```
    sCxt := Serialize(cxt)
```

`sCxt` is holding the byte array. To deserialize it:

```
    dsCxt := Deserialize(sCxt)
```

And now `dsCxt` can continue the execution from step 2:

```
    dsCxt.Run(true, -1)
```

The printed call stacks of both runs are shown below:


```
main 0, 

main 1, 
	double 0, n: 66

===================================

main 1, 
	double 1, n: 66, out: 132

main 1, outMain: 132

main 2, outMain: 132, foo: [5 70 100]

main 3, outMain: 5, foo: [5 70 100]
```

The calls above the double line are from the first run, and the calls
below the double line are from the second run. This call stack
is exactly the same for the unserialized program, if executed until
the end:

```
main 0, 

main 1, 
	double 0, n: 66

main 1, 
	double 1, n: 66, out: 132

main 1, outMain: 132

main 2, outMain: 132, foo: [5 70 100]

main 3, outMain: 5, foo: [5 70 100]
```

## Multiple Return Values

The base language can now handle multiple return values for
functions.

```
if fn, err := cxt.GetCurrentFunction(); err == nil {
    fn.AddOutput(MakeParameter("output1", i32))
    fn.AddOutput(MakeParameter("output2", i32))
}
```

The code above adds two int32 output parameters to the function
currently being modified.

Expressions had to be modified to handle multiple returns. a
CXExpression struct now holds an array of output names, which are used
to create the definitions in the stack at runtime. The number of
output names must be equal to the number of outputs of its operator.

## More Array Types

Arrays of the following types can now be created: byte ([]byte), int32
([]i32), int64 ([]i64), float32 ([]f32) and float64
([]f64). Arguments and definitions store the arrays in byte arrays
internally, and CX uses the type to decode the value when needed.

The evolutionary algorithm was modified to skip array operations while
mutating a solution, as these frequently resulted in out of bounds
errors.

## Compilation

Some basic optimizations can be performed on a program by using the
`Compile` method, e.g.:

```
cxt.Compile()
```

The CXProgram struct now holds a field representing a heap (a byte
array) which stores definitions of CX structs' fields. The `Compile`
method looks for every use of a struct field in the program, and
stores them in the heap. The found arguments are changed for offsets
and sizes which can be used to extract the values from the heap. The
process of accessing a struct's field has a complexity of O(NlogN) if
being interpreted. After calling `Compile`, the process has a
complexity of O(1).

A comparison between an interpreted and a compiled program was
performed by using the following code:

```
    fmt.Println("Interpreted")
	for i := 0; i < 30; i++ {
		start := time.Now()

		for j := 0; j < 10000; j++ {
			cxt.ResetTo(0)
			cxt.Run(false, -1)
		}
		
		elapsed := time.Since(start)
		fmt.Println(elapsed)
	}

	cxt.Compile()

    fmt.Println("Compiled")
	for i := 0; i < 30; i++ {
		start := time.Now()

		for j := 0; j < 10000; j++ {
			cxt.ResetTo(0)
			cxt.Run(false, -1)
		}
		
		elapsed := time.Since(start)
		fmt.Println(elapsed)
	}
```

The same program was ran 30 times to approximate a normal distribution
for the measured times, and a hypothesis test was performed.

The average execution time for the interpreted program
was 4.833766611, with a standard deviation of 0.020860135. The average
execution time for the compiled program was 4.817615965, with a
standard deviation of 0.02491697.

A T Student test (I used StatDisk, and it doesn't have Z-test AFAIK,
but the T test works) reveals that, with a confidence interval of 99%
(`tc = 2.66`), the interpretated program is statistically slower than
the compiled program with `t = 2.72`.

