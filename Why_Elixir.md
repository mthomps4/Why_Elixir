# Why Elixir?
>Elixir is a dynamic, functional language designed for building scalable and maintainable applications.

>Elixir leverages the Erlang VM, known for running low-latency, distributed and fault-tolerant systems, while also being successfully used in web development and the embedded software domain.

Elixir is 5 years old but built on top of 20+ years.

#### " " vs ' '
##### Strings " "

Elixir strings are UTF8 binaries, with all the raw speed and memory savings that brings. Elixir has a String module with Unicode functionality built-in and is a great example of writing code that writes code.

It’s still compiling to EVM bytecode at the end of the day.


```
iex> string = "hełło"
iex> byte_size(string)
7
iex> String.length(string)
5
```

UTF-8 requires one byte to represent the characters h, e, and o, but two bytes to represent ł. In Elixir, you can get a character’s code point by using ?:

```
iex> ?a
97
iex> ?ł
322
```

The string concatenation operation `<>` is actually a binary concatenation operator:

```
iex> <<0, 1>> <> <<2, 3>>
<<0, 1, 2, 3>>
```

A common trick in Elixir is to concatenate the null byte <<0>> to a string to see its inner binary representation:

```
iex> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>
```
#### Char lists -- ' '
A char list is nothing more than a list of code points. Char lists may be created with single-quoted literals:

```
iex> 'hełło'
[104, 101, 322, 322, 111]
iex> is_list 'hełło'
true
iex> 'hello'
'hello'
iex> List.first('hello')
104
```

> So while double-quotes represent a string (i.e. a binary), single-quotes represent a char list (i.e. a list)

>In practice, char lists are used mostly when interfacing with Erlang, in particular old libraries that do not accept binaries as arguments. You can convert a char list to a string and back by using the `to_string/1` and `to_charlist/1` functions:

Note that those functions are polymorphic. They not only convert char lists to strings, but also integers to strings, atoms to strings, and so on.

---

While you can’t escape using the Erlang standard library from time to time, Elixir’s standard library is trying to normalize zero-based access and noun-first argument ordering, along with a more consistent API.

```
elem({:a, :b, :c}, 0) #=> :a
List.member?([:a, :b, :c], :b) #=> true
```

#### Using Erlang
`:math`, `:binary`, etc..

[Erlang Libraries](https://elixir-lang.org/getting-started/erlang-libraries.html)

```
iex> angle_45_deg = :math.pi() * 45.0 / 180.0
iex> :math.sin(angle_45_deg)
0.7071067811865475
iex> :math.exp(55.0)
7.694785265142018e23
iex> :math.log(7.694785265142018e23)
55.0
```

#### Modules
Modules provide way to encapsulate functionality... as many modules as needed even in the same file ... functions macros records etc.

### Mix Tool

### Functional programming

>Functional programming promotes a coding style that helps developers write code that is short, fast, and maintainable.

>It shapes the way developers think about the world they’re modeling. An OO language will make us look for entities with state and behavior, while in FP language we’ll think about data and transformations.

> Immutability means that the code is easy to reason about, test, and if done properly, much easier to read. You have no surprise hidden mutation of state in random calls. You have no worry about how a variable is going to change as it is called. The program is easy to follow and read.

#### Piping

#### Pattern Matching

#### Protocals
> Protocols are the Elixir way of providing something roughly similar to OO interfaces. Protocols allow developers to create a generic logic that can be used with any type of data, assuming that some contract is implemented for the given data.

```
Something
Stream.
Enum.filter
Enum.each

```

### Test Driven
Doc Tests
Unit Tests

### Phoenix
Phoenix -- Deps -- QL --

> Phoenix is a modern web framework that makes building APIs and web applications easy. Since it’s built with Elixir and runs on that awesome Erlang VM, it’s fast as hell and has excellent support for handling very large numbers of simultaneous users.

### Resources
[The Excitment of Elixir](http://devintorr.es/blog/2013/01/22/the-excitement-of-elixir/)

[Elixir: It's Not About Syntax](http://devintorr.es/blog/2013/06/11/elixir-its-not-about-syntax/)

[Erlang Libraries](https://elixir-lang.org/getting-started/erlang-libraries.html)
