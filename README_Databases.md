# Talking to PostgreSQL Database from Phoenix Project

## Database with Postgrex (low level, driver)(mm)

* https://hexdocs.pm/postgrex/Postgrex.html#start_link/1
* https://hexdocs.pm/postgrex/Postgrex.html#query!/4


```
iex> {:ok, pid} = Postgrex.start_link(hostname: "localhost", username: "postgres", password: "postgres", database: "postgres")
{:ok, #PID<0.69.0>}

iex> Postgrex.query!(pid, "SELECT user_id, text FROM comments", [])
%Postgrex.Result{command: :select, empty?: false, columns: ["user_id", "text"], rows: [[3,"hey"],[4,"there"]], size: 2}}

iex> Postgrex.query!(pid, "INSERT INTO comments (user_id, text) VALUES (10, 'heya')", [])
%Postgrex.Result{command: :insert, columns: nil, rows: nil, num_rows: 1}}
```

## Database With Ecto (tp)

    mix ecto.gen.repo -r Questions.Repo

0. First add a schema: https://hexdocs.pm/ecto/Ecto.Schema.html#module-example
0. Then execute `Repo.get(SchemaName, id)`

## Trouble shooting

### Ecto reset

`MIX_ENV=test mix ecto.reset`

### Ecto: execute migration with Logs

`mix ecto.migrate --log-sql`

### Update your Database Repository by running Ecto Migrations:

`$ mix ecto.migrate`

### Manually Insert a Row in your Database Model

```bash
struct = %Operations.Topics.Topic{}
attr = %{title: "Hello, world!"}
changeset = Operations.Topics.change_topic(struct, attr)
Operations.Repo.insert(changeset)
```
