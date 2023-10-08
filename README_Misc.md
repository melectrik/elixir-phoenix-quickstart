# Pure Elixir Project w/ Mix Package Manager

## Create Project

`mix new projname --sup`

## Start Project

`mix -S iex`

## Add Lib or Package to a Project

0. edit `deps` adding `{:ex_doc, "~> 0.12"}` to `mix.exs`, then 
0. mix hex.organization auth "${HEXPM_ORGANIZATION}" --key "${HEXPM_API_KEY}"
0. mix deps.get # to install the package
0. MIX_ENV=prod mix release [NAME]
0. mix docs # to use the package

## Tests are first class citizen in Elixir

`mix test`


# Misc


## How to use asdf .too-versions for parallel language versions

```bash
# asdf plugin-add elixir https://github.com/asdf-vm/asdf-elixir.git
# asdf local elixir ref:<commit reference>
# asdf plugin add erlang https://github.com/asdf-vm/asdf-erlang.git
# asdf local erlang ref:basho

# example content (libero)
elixir 1.13.2-otp-24
erlang 24.2
nodejs 16.9.1
```

## How to identifying slow-compiling resources

```bash
mix compile --force --profile time
```

will tell you which files are being recompiled, so you can change a single file and run a compilation in verbose mode to see what files that causes to be recompiled

```bash
mix compile --verbose
```

identifying what dependencies a given file has

```bash
mix xref trace FILE
```

## Override  mix.exs when in trouble w/ library file path:

`MIX_EXS=infrastructure/mix-prod.exs mix deps.get`

Ref: https://thoughtbot.com/blog/running-project-mix-commands-from-any-directory

Also possible in `mix.exs` as package declaration:

```elixir
  def project do
    [
      app: :linters,
      version: "0.1.0",
      elixir: "~> 1.4",
      build_embedded: Mix.env == :prod,
      start_permanent: Mix.env == :prod,
      deps: deps(),
      lockfile: Path.expand("mix.lock", __DIR__),
      deps_path: Path.expand("deps", __DIR__),
      build_path: Path.expand("_build", __DIR__),
   ]
  end
```

## identify your longest dependency cycles

```bash
mix xref graph --format cycles
```

if too long loops then try this:

```bash
mix xref graph --format cycles | grep "Cycle of length" | sed 's/[^0-9]//g' | paste -sd+ - | bc
```


## stop dependency cycles re-occuring in your CI pipeline

don't introduce new dependency cycles:

```bash
mix xref graph --format cycles --fail-above X
```

### A very stupid hex debugging session

In the end it turned out that the package repository hex.pm wasn't responding correctly at that moment. So you can ignore this section.


Check and fix local tooling versions:

```bash
asdf list-all erlang | while read ver
do
  asdf uninstall erlang $ver
  asdf install erlang $ver
done

asdf list-all elixir | while read ver
do
  asdf uninstall elixir $ver
  asdf install elixir $ver
done
```

```
RUN curl https://repo.hex.pm/installs/hex-1.x.csv
curl -v https://repo.hex.pm/installs/rebar-1.x.csv
MIX_DEBUG=1 mix local.hex
host repo.hex.pm
nslookup repo.hex.pm
```


# References

* [Master Functional Programming techniques with Elixir and Phoenix while learning to build compelling web applications! - By Stephen Grider](https://www.udemy.com/course/the-complete-elixir-and-phoenix-bootcamp-and-tutorial/learn/lecture/5941706#overview)

# Building and Optimizing Elixir Builds

* https://medium.com/multiverse-tech/how-to-speed-up-your-elixir-compile-times-part-3-strategies-to-improve-your-compile-times-fc0c448b5558

## How To use auto start script

`.iex.exs` is a script to be auto loaded on server start.


## Next: Further Advanced Topics to go (ks)

* Genserver, Supervisor, auch Process, mit anderer Rolle. Hat
  * msg box
  * status
* start_link
* The Actor Model https://en.wikipedia.org/wiki/Actor_model
* Embedded Elixir: https://www.nerves-project.org/
