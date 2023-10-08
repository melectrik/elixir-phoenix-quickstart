# How To init an Elixir Phoenix Project - Quick Start

Follow these steps to very quickly create a `Phoenix` web project and start it in a web server context on `Erlang`'s VM.

```bash
# Create app:
mix local.hex
mix archive.install hex phx_new
mix phx.new operations
# deprecated: add conn to config/dev.exs. then:
cd operations

# this creates the Postgresql connection
# don't use if you don't want the database setup
mix ecto.create

mix hex.info # get infos
mix deps.get
```

```bash
# Start app server:
mix phx.server # or
iex -S mix phx.server
```

## Output from project init above

> We are almost there! The following steps are missing:


## Then configure your database in config/dev.exs and run:


```bash
$ cd operations
$ mix ecto.create
```

## Start your Phoenix app with:

```bash
$ mix phx.server
```

## You can also run your app inside IEx (Interactive Elixir) as:

Note: this is going to start the Erlang VM called `Beam` and start your server process inside it:

```bash
$ iex -S mix phx.server
```

## How To add a Model to a Project


```bash
mix phx.gen.html Topics Topic topics title:string

```

## How to add the resource to your browser scope (server side)

in lib/operations_web/router.ex:

    resources "/topics", TopicController

## Update your Database Repository by running Ecto Migrations:

`$ mix ecto.migrate`

## Manually Insert a Row in your Database Model

```bash
struct = %Operations.Topics.Topic{}
attr = %{title: "Hello, world!"}
changeset = Operations.Topics.change_topic(struct, attr)
Operations.Repo.insert(changeset)
```
