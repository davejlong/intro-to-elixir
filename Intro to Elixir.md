footer: ![](Pics/Twitter.png) @davejlong PGP (4FF9ED7C)
build-lists: true
theme: Fira, 3

## Introduction to Elixir
### Danbury.io Elixir Workshop

---

![](Pics/Dave.jpg)

## Dave Long

### Director of Development @ Cage Data

Husband & Father
Missionary to Uganda
[@davejlong](https://twitter.com/davejlong) on Twitter / GitHub / Keybase
[davejlong.com](https://ghost.davejlong.com) on Web

---

![fit](Pics/dod_hartford.png)

---

![inline](Pics/cagedata.png)

[cageit.us/downtime-matters](http://cageit.us/downtime-matters)

^ Cage Data is your source for managed services in CT, brining a devops approach to classical IT

---

## Intro to Elixir

1. What is Elixir?
2. What can I do with it?
3. Let's build something!

^ We'll spend the first half of the talk going over some of the details of what Elixir and OTP is, where they come from and what the really powerful features of Elixir can give you. We'll also look at what Twilio is really quickly for those that don't know.

---
^ Elixir is a great language built on top of the Erlang platform.

> Elixir is a dynamic, functional language designed for building scalable and maintainable applications.
-- [elixir-lang.org](https://elixir-lang.org)

* Scalable

^ Elixir's scalability comes from Erlang's treatment of concurrency as a first-class citizen.

* Fault-Tolerant

^ Elixir's Supervisor system provides a way to handle failures in processes running the application and give a huge toolset to control how processes should run.

* Functional

^ Elixir and Erlang of Functional programming languages focused on writing short, fast and maintainable code.

* Extensible

^ Elixir was designed with extensibility in mind. Using Elixir's macros makes it easy to do things like write your own DSLs.

---

# Fault-Tolerant

```elixir
import Supervisor.Spec

children = [
  supervisor(TCP.Pool, []),
  worker(TCP.Acceptor, [4040])
]

Supervisor.start_link(children, strategy: :one_for_one)
```

^ Applications fail. That's a hard truth of life. Elixir's supervisor system helps you control how to restart processes when they run into issues.

---

# Scalable

```elixir
current_process = self()

# Spawn an Elixir process (not an operating system one!)
spawn_link(fn ->
  send current_process, {:msg, "hello world"}
end)

# Block until the message is received
receive do
  {:msg, contents} -> IO.puts contents
end
```

^ Elixir applications != single-process giants. they're broken down into many processes.
The processes communicate with each other by sending messages

---

## Ruby and Python worry about concurrency

^ Ruby and Python are trying to figure out how to use all CPUs on a single machine

---
## Elixir and Erlang worry about distribution

^ Erlang built to distribute across lots of different systems
Concurrency is a side effect

---

# Functional

```elixir
def serve_drinks(%User{age: age}) when age >= 21, do # Code that serves drinks!

def serve_drinks(_), do: # Code that denies a drink

serve_drinks User.get("John Doe")
```

^ Elixir's pattern matching makes it easy to route logic around your application.

---

# What is Functional Programming

* Higher-order functions

^ Functions that take functions as arguments or return functions.

* Pure functions

^ Functions that have no side-effects. If I replaced the function with it's return value doesn't change the program.

* Recursion

^ Functions that perform iterations (loops) by calling themselves

* Immutable

^ Once a variable is set, the value can't be changed
Instead it is copied when it is changed and the old value is garbage collected

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Object Oriented Code

```ruby, [.highlight: 1-2]
title. # <= "My Awesome Blog Post!"
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

^ To make it a little clearer, let's look at an example of OOP vs FP

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Object Oriented Code

```ruby, [.highlight: 3]
title. # <= "My Awesome Blog Post!"
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Object Oriented Code

```ruby, [.highlight: 4]
title. # <= "My Awesome Blog Post!"
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Object Oriented Code

```ruby, [.highlight: 5]
title. # <= "My Awesome Blog Post!"
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

---

[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Object Oriented Code

```ruby
title. # <= "My Awesome Blog Post!"
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

## `my-awesome-blog-post`

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Functional Code

```elixir, [.highlight: 1-2]
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Functional Code

```elixir, [.highlight: 3]
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Functional Code

```elixir, [.highlight: 4]
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Functional Code

```elixir, [.highlight: 5]
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

# Functional Code

```elixir
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

## `my-awesome-blog-post`

---
[.footer]: Example source: https://robots.thoughtbot.com/elixir-for-rubyists

## Functional

```elixir
title # <= My Awesome Blog Post!
|> String.downcase
|> String.replace(~r/\W/, " ") # convert non-word chars (like -,!) into spaces
|> String.split                # drop extra whitespace
|> Enum.join("-")              # join words with dashes
```

## Object Oriented

```ruby
title. # <= My Awesome Blog Post!
  downcase.
  gsub(/\W/, " "). # convert non-word chars (like -,!) into spaces
  split.           # drop extra whitespace
  join("-")        # join words with dashes
```

---

# Cool!
## ...what can I do with it?

---

# Build web apps

* ![inline](Pics/phoenix.png)

^ Phoenix == The Rails of Elixir

* Plug

^ Plug ==  The Sinatra of Elixir

---

# Simple web-server on Plug

```elixir, [.highlight: 2, 4-5, 7-9, 11, 13-15]
defmodule MyRouter do
  use Plug.Router

  plug :match
  plug :dispatch

  get "/hello" do
    send_resp(conn, 200, "world")
  end

  forward "/users", to: UsersRouter

  match _ do
    send_resp(conn, 404, "oops")
  end
end
```

---

# Put it in an application

```elixir, [.highlight: 2-13]
defmodule MyApp do
  use Application

  def start(_type, _args) do
    import Supervisor.Spec

    children = [
      Plug.Adapters.Cowboy.child_spec(:http, MyRouter, [], [port: 4001])
    ]

    opts = [strategy: :one_for_one, name: MyApp.Supervisor]
    Supervisor.start_link(children, opts)
  end
end
```

---

# Other things to do

* Data processing
* Chat systems
* Neural networks
* Queuing systems

---

## Slides on GitHub
### [davejlong/intro-to-elixir](https://github.com/davejlong/intro-to-elixir)

---

# [.fit] Hack time!
