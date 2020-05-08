# Mac Setup

Setup your mac for Elixir and Phoenix

## HomeBrew

First, you need to have Homebrew. If not, install it from [Official site](https://brew.sh).

## asdf

Second, we are using [asdf](https://asdf-vm.com/#/core-manage-asdf-vm) to as version manager. Install it using Homebrew:

```sh
brew install asdf
```

After that, in your ~/.zshrc, add this line:

```txt
. $(brew --prefix asdf)/asdf.sh
```

Save changes and restart Terminal, OR run:

```sh
source ~/.zshrc
```

Now, you can verify asdf installation by typing:

```sh
asdf --version

<your_version>
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

(Optional) If you encounter Java dependency problem (due to [kerl](https://github.com/kerl/kerl)), you can install Java first, then install Erlang, OR you can skip Java dependency by referring this [Github issue](https://github.com/asdf-vm/asdf-erlang/issues/58):

```sh
brew cask install java
```

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

