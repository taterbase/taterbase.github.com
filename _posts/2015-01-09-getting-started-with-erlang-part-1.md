---
layout: post
title: Getting Started With Erlang (Part 1 - Semantics)
---

I've recently decided to start learning [Erlang](http://erlang.org) and I've always felt the best way to learn something is to teach others. My favorite style of teaching is pairing and face to face instruction but I don't have any mentees nearby interested in Erlang so I've decided to use blogging as a way of helping others and hopefully solidifying these concepts for myself. Erlang's syntax is very interesting and a little alien so bear with me as I try to make sense and explain the mechanics. That being said, let's get on with the post!

## Installation
I won't spend too much time talking about installing Erlang other than to say I'm using a Mac and you can install with [Homebrew](http://brew.sh) simply by running the `brew install erlang` command. Erlang should be available for most Linux distributions as well.

## The REPL
Erlang comes with a [REPL](http://en.wikipedia.org/wiki/Read–eval–print_loop) that you can start up with the command `erl`. You can run simple arithmetic like most REPL's such as `2 + 2` or `(5 + 3) * 2` but the key thing to remember is *almost* every Erlang statement must end with a period(`.`). Here's an example of some basic commands.

```
%> erl
Erlang/OTP 17 [erts-6.3] [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

Eshell V6.3  (abort with ^G)
1> 2+2.
4
2> (5 + 3) * 2.
16
3>
```

You'll use the REPL to compile and run your different Erlang programs. Speaking of which let's create a new simple program now.

## Modules
Each Erlang file describes a module and exports the functions necessary to interact with it. The module name needs to match the file name so if you create a `hello-world.erl` you'll need to name the module `hello-world` in the program, like this:

```erlang
-module('hello-world').
```

Notice the `.` at the end; it's important to end all statements like that. Don't worry if you forget though because the compiler will let you know something is funny and give you tips on how to correct it. Now you can hop in the REPL and compile your file:

```
1> c('hello-world').
{ok,'hello-world'}
```

Awesome, we got a [tuple](http://en.wikipedia.org/wiki/Tuple) back saying `ok` and the name of the module we just compiled. If our file name didn't match up with our module name the compiler would have thrown an error. If instead of `-module('hello-world').` I had written `-module('goodbye-world').` the compiler would have said something like this:

```
5> c('hello-world').
hello-world.beam: Module name 'goodbye-world' does not match file name 'hello-world'
error
```

When we successfully compiled the file notice how `ok` isn't surrounded in single quotes? `ok` is what's called an atom. In fact, so is `'hello-world'`.

## Atoms
Atoms are literal constants within Erlang and can be passed around as an expressive piece of information. Atoms can either be described using words starting with a lowercase letter or surrounded in single quotes to use capital letters and non-alphanumeric characters. Here are some examples of atoms:

```erlang
apple
cat
'Marmalade'
'hello-world'
```

Atoms don't hold a value, they simply provide a consistent expressive way for representing information. They're closely related to [enums](http://en.wikipedia.org/wiki/Enumerated_type) and [symbols](http://en.wikipedia.org/wiki/Symbol_(programming)).

## Functions and Variables
So we've compiled our first Erlang file but it doesn't do anything yet. Let's create a simple function to double a number's value. Function names, much like atoms, are defined starting with a lowercase letter with some parameters defined. 

```erlang
myfunc(SomeVar) ->
```

Variables on the other hand begin with an uppercase letter. Not only that but a variable can only be defined **once**. This one of many properties in Erlang that make it a [functional language](http://en.wikipedia.org/wiki/Functional_programming) and is included to remove the possibility of [side-effects](http://en.wikipedia.org/wiki/Side_effect_(computer_science)). If you want to save the result of a calculation you'll need to store it in a new variable.

```
11> MyVar = 3,
11> MyOtherVar = MyVar + 3.
6
```

See that comma I threw in there? When you're building a function the period is for the very end of the function and commas are used to separate statements leading up to the final statement. We'll talk more about that later, for now let's go ahead and create our double function.

```erlang
-module('hello-world').

double(Val) ->
	Val * 2.
```

That's fairly simple isn't it? We created a function that accepts a value and assigns it to `Val`. We then multiply it by `2`. Notice that we don't have to explicitly call `return`. Erlang implicitly returns the last evaluated expression from each function. We could even do this:

```erlang
-module('hello-world').

double(Val) ->
	DoubledValue = Val * 2,
	DoubledValue.
```

There's that comma again, separating the first statement from the last. As I said before, the comma says there are more statements to come, I'm not yet done processing. Also this time we return the value by simply putting the result as the last statement. Erlang will evaluate and return it just the same.

This function is looking pretty solid and all that's left is to export it so it can be used. You'll use the `-export()` statement to do that.

```erlang
-module('hello-world').
-export([double/1]).

double(Val) ->
	DoubledValue = Val * 2,
	DoubledValue.
```

`-export` takes an array of function definitions you would like to export. Function names need to be accompanied by their [arity](http://en.wikipedia.org/wiki/Arity) which is the number of arguments they accept. Functions with the same name but different arity are considered completely separate. Now that we've exported `double` we can compile the file and use the function.

```
18> c('hello-world').
{ok,'hello-world'}
19> 'hello-world':double(3).
6
```

Success! 

That's all I'll write for this installment but if you like what you've seen so far you can [follow me on Twitter](https://twitter.com/taterbase) or [subscribe to my RSS feed](http://georgeshank.com/feed.xml) and you'll know when the next blog post in this Erlang series gets posted.
