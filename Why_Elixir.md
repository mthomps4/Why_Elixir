# Why Elixir?
>Elixir is a dynamic, functional language designed for building scalable and maintainable applications.

>Elixir leverages the Erlang VM, known for running low-latency, distributed and fault-tolerant systems, while also being successfully used in web development and the embedded software domain.

Elixir is 6 years.. Erlang has 20+ years.

#### Using Erlang with Elixir
Accessing erlang in Elixir

`:erlang.`


[Erlang Libraries](https://elixir-lang.org/getting-started/erlang-libraries.html)

`:math`, `:binary`, etc..

```elixir
iex> angle_45_deg = :math.pi() * 45.0 / 180.0
iex> :math.sin(angle_45_deg)
0.7071067811865475
iex> :math.exp(55.0)
7.694785265142018e23
iex> :math.log(7.694785265142018e23)
55.0
```

-- For Erlang Questions @ Brian

### Mix Tool
Mix is a build tool that provides tasks for creating, compiling, and testing Elixir projects, managing its dependencies, and more.

[Mix Hex Docs](https://hexdocs.pm/mix/Mix.html)

`mix new` - creates a new elixir project

`mix compile` - compiles the current project

`mix test` - runs tests for the given project

`mix run` - runs a particular command inside the project

`mix deps` - List dependencies and status

`mix deps.get` - get out of date dependencies

### Functional programming
Elixir is Not Object Oriented, It's Process Oriented

> Functional programming promotes a coding style that helps developers write code that is short, fast, and maintainable.

> It shapes the way developers think about the world they’re modeling. An OO language will make us look for entities with state and behavior, while in FP language we’ll think about data and transformations.

> Immutability means that the code is easy to reason about, test, and if done properly, much easier to read. You have no surprise hidden mutation of state in random calls. You have no worry about how a variable is going to change as it is called. The program is easy to follow and read.

A Note About Immutable Variables
Elixir is a functional language which means that variables can’t change value. You can, however, reuse variable names. If you set x = 2 and then set x = 3, Elixir doesn’t mind.

### Documentation
[Writing Documentation](https://hexdocs.pm/elixir/writing-documentation.html#content)

Hex Docs can be created from internal documentation. Examples within the Documentation can also be included within Mix Tests. Writing doc tests reminds the developer to write smaller more precise in/out functions.
Having these examples tested also gives a good starting point when something has been changed and needs updating.

##### Modules
Modules provide a way to encapsulate functionality... as many modules as needed even in the same file ... functions macros records etc.

```elixir
defmodule MyModule.Some.Helper do
  @doc "Hello world"
  def hello do
    IO.puts "Hello, World!"
  end
end

iex> MyModule.Some.Helper.hello
"Hello, World!"

iex> alias MyModule.Some.Helper
iex> Helper.hello
"Hello, World!"
```

#### Protocals and Piping
> Protocols are the Elixir way of providing something roughly similar to OO interfaces. Protocols allow developers to create a generic logic that can be used with any type of data, assuming that some contract is implemented for the given data.

```elixir
iex> Enum.map([1, 2, 3], fn x -> x * 2 end)
[2, 4, 6]
```

```elixir
"hello, world!"
|> String.split(" ")
|> Enum.map(&String.capitalize/1)
|> Enum.join

"Hello, World!"
```

#### Pattern Matching and Error Handling
```elixir
iex> {a, b, _} = {:hello, “world”, 42}
{:hello, “world”, 42}
```

```elixir
iex> {:hello, b, c} = {:hello, “world”, 42}
{:hello, “world”, 42}
iex> b
“world”
iex> c
42
```

```elixir
iex> {result, value} = {:ok, 2}
{:ok, 2}
iex> result
:ok
iex> value
2
```

```elixir
# Lists
iex> list = [1, 2, 3]
iex> [1, 2, 3] = list
[1, 2, 3]
iex> [] = list
** (MatchError) no match of right hand side value: [1, 2, 3]
```

How this helps!

```elixir
case MyApp.cool_function(param) do
 {:ok, result} -> result
 {:error, message} -> raise "Error in cool_function with param: #{param} -- Error: #{message}"
 true -> "Catch all Error"
end
```

```elixir
case MyApp.cool_function(param) do
 {:ok, result} ->
  result
  |> do_stuff()

 {:error, message} -> raise "Error in cool_function with param: #{param} -- Error: #{message}"

 true -> "Catch all Error"
end
```

#### Pattern Matching and Guards
Pattern Matching with Functions
```elixir
defmodule Chatter do
  def converse({:hello, name, employer}) do
    IO.puts “Hi #{name}. Nice to meet you. I hear you work for #{employer}.”
  end

  def converse({:small_talk, name, fav_hobby}) do
    IO.puts “Hey #{name}, have you been doing much #{fav_hobby} lately?”
  end

  def converse({:goodbye, name}) do
    IO.puts “#{name}, great to talk to you today, goodbye.”
  end
end
```

```elixir
iex> Chatter.converse({:hello, Matt, Eltoro})
iex> “Hi Matt. Nice to meet you. I hear you work for Eltoro.”
```

##### Guards

```elixir
def my_function(number) when is_even(number) do
  # do stuff
end

def my_function(number) do
  # do stuff
end
```

```elixir
def my_function(param) when is_bitstring(param) do
  map = case Poison.decode(param) do
    {:ok, result} -> result
    {:error, message} -> raise "Error: #{message}"
  end

  my_function(map)
end

def my_function(param) when is_map(param) do
  # do real work
end
```

#### " " vs ' '
##### Strings " "

> Elixir strings are UTF8 binaries, with all the raw speed and memory savings that brings. Elixir has a String module with Unicode functionality built-in.

It’s still compiling to EVM bytecode at the end of the day.

```elixir
iex> string = "hełło"
iex> byte_size(string)
7
iex> String.length(string)
5
```

UTF-8 requires one byte to represent the characters h, e, and o, but two bytes to represent ł. In Elixir, you can get a character’s code point by using ?:

```elixir
iex> ?a
97

iex> ?ł
322
```

The string concatenation operation `<>` is actually a binary concatenation operator:

```elixir
iex> <<0, 1>> <> <<2, 3>>
<<0, 1, 2, 3>>
```

A common trick in Elixir is to concatenate the null byte <<0>> to a string to see its inner binary representation:

```elixir
iex> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>
```
#### Char lists -- ' '
A char list is nothing more than a list of code points. Char lists may be created with single-quoted literals:

``` elixir
iex> 'hełło'
[104, 101, 322, 322, 111]
iex> is_list 'hełło'
true
iex> 'hello'
'hello'
iex> List.first('hello')
104
```

So while double-quotes represent a string (i.e. a binary), single-quotes represent a char list (i.e. a list)

> In practice, char lists are used mostly when interfacing with Erlang, in particular old libraries that do not accept binaries as arguments. You can convert a char list to a string and back by using the `to_string/1` and `to_charlist/1` functions:

Note that those functions are polymorphic. They not only convert char lists to strings, but also integers to strings, atoms to strings, and so on.

---

While you can’t escape using the Erlang standard library from time to time, Elixir’s standard library is trying to normalize zero-based access and noun-first argument ordering, along with a more consistent API.

```elixir
elem({:a, :b, :c}, 0) #=> :a
List.member?([:a, :b, :c], :b) #=> true
```

### Test Driven
Doc Tests
```elixir
@doc """
add_function takes an integer and adds 1

## Examples

    iex> MyApp.Module.add_function(2)
    3

"""
def add_function(a) when is_integer(a) do
  a + 1
end
```

Real Test - `ExUnit`

```elixir
defmodule Onspotgateway.HelperTest do
  use Onspotgateway.DataCase
# Mock Data
  alias Onspotgateway.Mock
# Importing
  alias Onspotgateway.Requestor.Helpers

  describe "expect an error" do
    refute {:ok, _} = func(bad_param) # returns {:error, _}
  end

  describe "requestor_helper" do
  test "normalize_polygon with valid wkb" do
    data = Mock.event_data_wkb_polygons()
    poly = List.first(data["polygons"])
    assert Helpers.normalize_polygon(poly) == %{
      "geometry" => %{
        "type" => "MultiPolygon",
        "coordinates" => [[[
          [-84.46623643976636, 33.92313999877337],
          [-84.46629544836469, 33.92277276374059],
          [-84.46631958824582, 33.92252793950595],
          [-84.46611574036069, 33.922510134079616],
          [-84.46587165934034, 33.92264590036137],
          [-84.46585020166822, 33.92275718403754],
          [-84.46580460411496, 33.922861790560574],
          [-84.46611305815168, 33.923340308124025],
          [-84.46641346556135, 33.92337814428182],
        ]]],
      }
    }
  end
end
```

### Macros and Metaprogramming

#### See Chris -- aka Doc
#### REPO Example
[portal backend experiments: Eltoroporta.API.Resolver](https://github.com/eltorocorp/portal-backend-experiments/blob/dev/lib/eltoroportal/api/Resolver.ex)

- Macros are a powerful construct and Elixir provides many mechanisms to ensure they are used responsibly.

- Macros are hygienic: by default, variables defined inside a macro are not going to affect the user code. Furthermore, function calls and aliases available in the macro context are not going to leak into the user context.

- Macros are lexical: it is impossible to inject code or macros globally. In order to use a macro, you need to explicitly require or import the module that defines the macro.

- Macros are explicit: it is impossible to run a macro without explicitly invoking it. For example, some languages allow developers to completely rewrite functions behind the scenes, often via parse transforms or via some reflection mechanisms. In Elixir, a macro must be explicitly invoked in the caller during compilation time.

- Macros’ language is clear: many languages provide syntax shortcuts for quote and unquote. In Elixir, we preferred to have them explicitly spelled out, in order to clearly delimit the boundaries of a macro definition and its quoted expressions.

  ```elixir
  defmodule MyModule do
    defmacro my_macro(a, b, c) do
      quote do
        # Keep what you need to do here to a minimum
        # and move everything else to a function
        do_this_that_and_that(unquote(a), unquote(b), unquote(c))
      end
    end

    def do_this_that_and_that(a, b, c) do
      do_this(a)
      ...
      do_that(b)
      ...
      and_that(c)
    end
  end
  ```

### Other parts of the tech stack
#### Phoenix
[Phoenix](http://phoenixframework.org/)
[Phoenix Hex Docs](https://hexdocs.pm/phoenix/Phoenix.html)
> Phoenix leverages the Erlang VM ability to handle millions of connections alongside Elixir's beautiful syntax and productive tooling for building fault-tolerant systems.

> Phoenix is a modern web framework that makes building APIs and web applications easy. Since it’s built with Elixir and runs on that awesome Erlang VM, it’s fast as hell and has excellent support for handling very large numbers of simultaneous users.

- [Phoenix HTML]() - conveniences for working with HTML in Phoenix
- [Plug]() - a specification and conveniences for composable modules in between web applications


#### Ecto
[Ecto](https://hexdocs.pm/ecto/Ecto.html) - a language integrated query and database wrapper

- Ecto.Repo - repositories are wrappers around the data store. Via the repository, we can create, update, destroy and query existing entries. A repository needs an adapter and credentials to communicate to the database

- Ecto.Schema - schemas are used to map any data source into an Elixir struct. We will often use them to map tables into Elixir data but that’s one of their use cases and not a requirement for using Ecto

- Ecto.Changeset - changesets provide a way for developers to filter and cast external parameters, as well as a mechanism to track and validate changes before they are applied to your data

- Ecto.Query - written in Elixir syntax, queries are used to retrieve information from a given repository. Queries in Ecto are secure, avoiding common problems like SQL Injection, while still being composable, allowing developers to build queries piece by piece instead of all at once

#### Ecto Changesets
Changesets allow filtering, casting, validation and definition of constraints when manipulating structs.

```elixir
defmodule User do
  use Ecto.Schema
  import Ecto.Changeset

  schema "users" do
    field :name
    field :email
    field :age, :integer
  end

  def changeset(user, params \\ %{}) do
    user
    |> cast(params, [:name, :email, :age])
    |> validate_required([:name, :email])
    |> validate_format(:email, ~r/@/)
    |> validate_inclusion(:age, 18..100)
    |> unique_constraint(:email)
  end
end


changeset = User.changeset(%User{}, %{age: 0, email: "mary@example.com"})
{:error, changeset} = Repo.insert(changeset)
changeset.errors # => [age: {"is invalid", []}, name: {"can't be blank", []}]

 ---

%Ecto.Changeset{action: nil, changes: %{name: "Mary", age: 10},
constraints: [], errors: [], filters: %{} ...}
```

### Resources
-- Hex Docs --
-- Elixir-lang.org --

[The Excitment of Elixir](http://devintorr.es/blog/2013/01/22/the-excitement-of-elixir/)

[Elixir: It's Not About Syntax](http://devintorr.es/blog/2013/06/11/elixir-its-not-about-syntax/)

[Erlang Libraries](https://elixir-lang.org/getting-started/erlang-libraries.html)
