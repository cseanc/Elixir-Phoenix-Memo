# Mac Setup for Elixir and Phoenix

Setup your mac for Elixir and Phoenix

## HomeBrew

First, you need to have Homebrew. If not, install it from [Official site](https://brew.sh).

## asdf

Second, we are using [asdf](https://asdf-vm.com/#/core-manage-asdf-vm) as the version manager for Elixir and Erlang. Install it using Homebrew:

```sh
brew install asdf
```

After that, in your `~/.zshrc`, add this line:

```sh
. $(brew --prefix asdf)/asdf.sh
```

Save changes and restart Terminal, OR run:

```sh
source ~/.zshrc
```

Now, you can verify asdf installation by typing:

```sh
asdf --version

v0.7.8
```

## Erlang

Third, we need to install Erlang runtime. asdf manages each component as a plugin. So we'll add Erlang as a plugin to asdf:

```sh
asdf plugin add erlang
```

Then, install latest stable version of Erlang:

```sh
asdf install erlang latest
```

>  (Optional) If you encounter Java dependency problem (due to [kerl](https://github.com/kerl/kerl)), you can install Java first, then install Erlang:

```sh
brew cask install adoptopenjdk
```

> OR you can skip Java dependency by referring this [Github issue](https://github.com/asdf-vm/asdf-erlang/issues/58).

This will take a while. After installation, activate Erlang globally:

```sh
asdf global erlang <current_version>
```

## Elixir

Fourth, we will install Elixir the same way. Add Elixir as a plugin:

```sh
asdf plugin add elixir
asdf install elixir latest
asdf global elixir <current_version>
```

You can verify Erlang and Elixir installation by:

```sh
elixir --version

Erlang/OTP 22 [erts-10.7.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe]

Elixir 1.10.3 (compiled with Erlang/OTP 22)
```

(Optional) Try the interpreter `iex`:

```sh
iex

Interactive Elixir (1.10.3) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> IO.puts("Hello world")
Hello world
:ok
iex(2)> 
```

## Phoenix

Fifth, we will install Phoenix using `hex`, the package manager for Elixir. We will install `hex` using Elixir executable `mix` that comes along with your Elixir installation:

```
mix --version

Erlang/OTP 22 [erts-10.7.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [hipe]

Mix 1.10.3 (compiled with Erlang/OTP 22)
```

Install `hex` package manager:

```sh
mix local.hex
```

Install Phoenix:

```sh
mix archive.install hex phx_new
```

## Node.js

`node.js` is a dependency because Phoenix by default use `webpack` to compile static assets. I recommend using `nvm` version manager to manage your `node.js` installation.

Install [nvm](https://github.com/nvm-sh/nvm):

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

Add this line in your `~/.zshrc`:

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

Then, restart Terminal OR run:

```sh
source ~/.zshrc
```

Install latest LTS version of `node.js`:

```sh
nvm install --lts
```

Verify your installation:

```sh
which node

<HOME_PATH>/.nvm/versions/node/v12.16.1/bin/node
```

## Postgresql

I normally install Postgresql database by following the [Rails](https://gorails.com/setup) guide:

```sh
brew install postgresql

# To have launchd start postgresql at login:
brew services start postgresql
```

> This installation will auto-generate a user same name with your current Mac login user, with no password. If you encounter Phoenix authentication error trying to create development database, see the next section.

# Trying It Out

Now you should have everything installed. We can try creating a dummy Phoenix project:

```sh
mix phx.new hey
```

Enter `y` for the install dependencies question.

After installation finish, we need to config `config/dev.exs` to match our Homebrew installation of Postgresql. Navigate to `<project_root>/config/dev.exs` and change the username and empty out the password:

```elixir
# Configure your database
config :hey, Hey.Repo,
  username: "<change_to_your_username>",
  password: "",
  database: "hey_dev",
  hostname: "localhost",
  show_sensitive_data_on_connection_error: true,
  pool_size: 10

```

Save the changes, then create the development database using `ecto`:

```sh
cd <project_name>

mix ecto.create
```

Enter `y` for the rebar3 installation question.

Finally, serve the Phoenix app:

```sh
mix phx.server
```

Visit your website at `http://localhost:4000`. 🎈

