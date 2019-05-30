# Elixir

## Basics

- Guards 的错误不会抛出只会让对应的 guard 验证失效

- 匿名函数的子句的参数个数必须一致，否则会报错

- 可以将 string concat 到空字节 <<0>> 来看它的内部二进制表示

- Keyword lists have three special characteristics: Keys must be atoms. Keys are ordered, as specified by the developer. Keys can be given more than once. Maps allow any value as a key. Maps’ keys do not follow any ordering.

- Although we can pattern match on keyword lists, it is rarely done in practice since pattern matching on lists requires the number of items and their order to match.

- When all the keys in a map are atoms, you can use the keyword syntax for convenience: %{a: 1, b: 2}

- 可以用 map.key 的方式来 accessing atom keys

- This file can be compiled using elixirc: `elixirc math.ex`. This will generate a file named `Elixir.Math.beam` containing the bytecode for the defined module. If we start `iex` again, our module definition will be available (provided that `iex` is started in the same directory the bytecode file is in).

- Elixir projects are usually organized into three directories:

  - `ebin`: contains the compiled bytecode
  - `lib`: contains elixir code (usually  .ex  files)
  - `test`: contains tests (usually  .exs  files)

  When working on actual projects, the build tool called `mix` will be responsible for compiling and setting up the proper paths for you. For learning purposes, Elixir also supports a scripted mode which is more flexible and does not generate any compiled artifacts. In addition to the Elixir file extension `.ex`, Elixir also supports `.exs` files for scripting. Elixir treats both files exactly the same way, the only difference is in intention. `.ex` files are meant to be compiled while `.exs` files are used for scripting. When executed, both extensions compile and load their modules into memory, although only `.ex` files write their bytecode to disk in the format of `.beam` files.

- 通过 `\\` 来定义的函数参数的缺省值或表达式是在被调用时才被 evaluated 的，而不是在定义函数的时候

- All the functions in the `Enum` module are eager. As an alternative to `Enum`, Elixir provides the `Stream` module which supports lazy operations. Instead of generating intermediate lists, streams build a series of computations that are invoked only when we pass the underlying stream to the `Enum` module. Streams are useful when working with large, possibly infinite, collections.

- Structs are extensions built on top of maps that provide compile-time checks and default values. Structs take the name of the module they’re defined in. Underneath structs are bare maps with a fixed set of fields. As maps, structs store a "special" field named `__struct__` that holds the name of the struct. Notice that we referred to structs as **bare** maps because none of the protocols implemented for maps are available for structs. For example, you can neither enumerate nor access a struct. However, since structs are just maps, they work with the functions from the `Map` module. Structs alongside protocols provide one of the most important features for Elixir developers: data polymorphism.

- 实现了 `String.Chars` 协议的类型可以直接打印成字符串，when there is a need to "print" a more complex data structure, one can use the `inspect` function, based on the `Inspect` protocol. The `Inspect` protocol is the protocol used to transform any data structure into a readable textual representation. This is what tools like IEx use to print results. Keep in mind that, by convention, whenever the inspected value starts with  # , it is representing a data structure in non-valid Elixir syntax. This means the inspect protocol is not reversible as information may be lost along the way.

- A common use case of `:into` can be transforming values in a map, without touching the keys:

  ```sh
  iex> for {key, val} <- %{"a" => 1, "b" => 2}, into: %{}, do: {key, val * val}
  %{"a" => 1, "b" => 4}
  ```

- Let’s make another example using streams. Since the IO module provides streams (that are both `Enumerable`s and `Collectable`s), an echo terminal that echoes back the upcased version of whatever is typed can be implemented using comprehensions:

  ```sh
  iex> stream = IO.stream(:stdio, :line)
  iex> for line <- stream, into: stream do
  ...>   String.upcase(line) <> "\n"
  ...> end
  ```

- The `after` clause will be executed regardless of whether or not the tried block succeeds. Note, however, that if a linked process exits, this process will exit and the `after` clause will not get run. Thus `after` provides only a soft guarantee. Luckily, files in Elixir are also linked to the current processes and therefore they will always get closed if the current process crashes, independent of the `after` clause. You will find the same to be true for other resources like ETS tables, sockets, ports and more.

- Sometimes you may want to wrap the entire body of a function in a `try` construct, often to guarantee some code will be executed afterwards. In such cases, Elixir allows you to omit the `try` line.

- The `:crypto` module is not part of the Erlang standard library, but is included with the Erlang distribution. This means you must list `:crypto` in your project’s applications list whenever you use it. To do this, edit your `mix.exs` file to include it in `extra_applications`.

- Naming dynamic processes with atoms is a terrible idea! If we use atoms, we would need to convert the bucket name (often received from an external client) to atoms, and we should never convert user input to atoms. This is because atoms are not garbage collected. Once an atom is created, it is never reclaimed. Generating atoms from user input would mean the user can inject enough different names to exhaust our system memory! In practice, it is more likely you will reach the Erlang VM limit for the maximum number of atoms before you run out of memory, which will bring your system down regardless.

- A GenServer is implemented in two parts: the client API and the server callbacks. You can either combine both parts into a single module or you can separate them into a client module and a server module. The client and server run in separate processes, with the client passing messages back and forth to the server as its functions are called. Observe that we were able to considerably change the server implementation without changing any of the client API. That’s one of the benefits of explicitly segregating the server and the client. There are two types of requests you can send to a GenServer: calls and casts. Calls are synchronous and the server must send a response back to such requests. Casts are asynchronous and the server won’t send a response back.

- The advantage of using `start_supervised!` is that ExUnit will guarantee that the registry process will be shutdown before the next test starts. In other words, it helps guarantee the state of one test is not going to interfere with the next one in case they depend on shared resources. When starting processes during your tests, we should always prefer to use `start_supervised!`. Internally, the `start_supervised!` function starts the registry under a supervisor defined by the ExUnit framework.

- Note `Process.monitor(pid)` returns a unique reference that allows us to match upcoming messages to that monitoring reference.

- `handle_info/2` must be used for all other messages a server may receive that are not sent via `GenServer.call/2` or `GenServer.cast/2`, including regular messages sent with `send/2`. The monitoring `:DOWN` messages are such an example of this. Since any message, including the ones sent via `send/2`, go to `handle_info/2`, there is a chance unexpected messages will arrive to the server. Therefore, if we don't define the catch-all clause, those messages could cause our registry to crash, because no clause would match. We don’t need to worry about such cases for `handle_call/3` and `handle_cast/2` though. Calls and casts are only done via the `GenServer` API, so an unknown message is quite likely a developer mistake.

- Links are bi-directional. If you link two processes and one of them crashes, the other side will crash too (unless it is trapping exits). A monitor is uni-directional: only the monitoring process will receive notifications about the monitored one. In other words: use links when you want linked crashes, and monitors when you just want to be informed of crashes, exits, and so on.

- When you run `iex -S mix`, it is equivalent to running `iex -S mix run`. So whenever you need to pass more options to Mix when starting IEx, it’s a matter of typing `iex -S mix run` and then passing any options the `run` command accepts.

- Mix makes a distinction between projects and applications. When we say “project” you should think about Mix. Mix is the tool that manages your project. It knows how to compile your project, test your project and more. It also knows how to compile and start the application relevant to your project. When we talk about applications, we talk about OTP. Applications are the entities that are started and stopped as a whole by the runtime. You can learn more about applications and how they relate to booting and shutting down of your system as a whole in the docs for the Application module.

- You may be wondering why use a supervisor if it never restarts its children. It happens that supervisors provide more than restarts, they are also responsible to guarantee proper startup and shutdown, especially in case of crashes in a supervision tree. At the end of the day, tools like Observer is one of the reasons you want to always start processes inside supervision trees, even if they are temporary, to ensure they are always reachable and introspectable.

- `mix test --trace`: the `--trace` option is useful when your tests are deadlocking or there are race conditions, as it runs all tests synchronously (`async: true` has no effect) and shows detailed information about each test.

- Keep in mind that applications in an umbrella project all share the same configurations and dependencies. If two applications in your umbrella need to configure the same dependency in drastically different ways or even use different versions, you have probably outgrown the benefits brought by umbrellas. Remember you can break the umbrella and still leverage the benefits behind "mono-repos".
