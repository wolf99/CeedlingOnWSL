# Ceedling On Windows Subsystem for Linux (WSL)
How to install 
[Ceedling](https://github.com/ThrowTheSwitch/Ceedling/blob/master/README.md) 
or [CMock](https://github.com/ThrowTheSwitch/CMock/blob/master/README.md) 
on WSL using [Ruby](https://www.ruby-lang.org/en/documentation/installation/#apt) 
and [rbenv](https://github.com/rbenv/rbenv/blob/master/README.md).

The Unity testing framework from ThrowtheSwitch.org can be run using 
rake with a regular Ruby install (`$ sudo apt-get install ruby-all`).
But Cmock and Ceedling require gems to be installed using bundler. 
This presents a problem with regards to permissions on WSL/Ubuntu.
To overcome this we use Ruby in rbenv instead of a normal Ruby install. 
I have tried using RVM but suffered defeat in permissions hell.

**Compatibility note**: rbenv is _incompatible_ with RVM. Please make
  sure to fully uninstall RVM and remove any references to it from
  your shell initialization files before installing rbenv.
  
## Before beginning ##
Where I say restart the terminal I mean exit from bash and exit from
the terminal also. I have found that sometimes only an exit from bash 
is required but this appears inconsistent and it makes for a smoother 
install to take every precaution!

## To Begin ##
If you have already installed Ruby I would recommend uninstalling it. 
This step is not required but reduces potential problems as all the 
Rubys installed on a system are relative to the same location (i.e. 
rbenv). You can do this with a simple 

~~~ sh
$ sudo apt-get purge ruby
~~~

Then make sure any mentions of Ruby in `~/.bash_profile`, `~/.bashrc` 
and so on are removed also. 

After doing this restart your terminal.

## Installing rbenv 
rbenv has a simple install process on it's github repo 
[readme.md](https://github.com/rbenv/rbenv/blob/master/README.md). 
The steps I used from this are outlined below:

1. Check out rbenv into `~/.rbenv`.

    ~~~ sh
    $ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
    ~~~

    Optionally, try to compile dynamic bash extension to speed up rbenv. 
    Don't worry if it fails; rbenv will still work normally:

    ~~~
    $ cd ~/.rbenv && src/configure && make -C src
    ~~~

2. Add `~/.rbenv/bin` to your `$PATH` for access to the `rbenv`
   command-line utility.

    ~~~ sh
    $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    ~~~

    **Zsh note**: Modify your `~/.zshrc` file instead of `~/.bashrc`.

3. Add the following to your `~/.bashrc` to initialize rbenv to enable 
   shims and autocompletion.
   
   ~~~ sh
   $ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
   ~~~

4. Restart your terminal so that PATH changes take effect. Now check 
   if rbenv was set up:

    ~~~ sh
    $ type rbenv
    #=> "rbenv is a function"
    ~~~

5.  Install [ruby-build](https://github.com/rbenv/ruby-build#readme), 
    which provides the `rbenv install` command that simplifies the 
    process of installing new Ruby versions.
    
### Installing a version of Ruby ##
The `rbenv install` command doesn't ship with rbenv out of the box, but
is provided by the [ruby-build](https://github.com/rbenv/ruby-build#readme) 
project. If you installed it either as part of GitHub checkout process 
outlined above or via Homebrew, you should be able to:

~~~ sh
# list all available versions:
$ rbenv install -l

# install a Ruby version:
$ rbenv install 2.3.1
~~~

## Selecting a Ruby version for use ##
See the currently installed Ruby versions using `$ rbenv versions`.

See the currently selected Ruby version using `$ rbenv version`.

Change the selected version using `$ rbenv local [version]`.

## Running Ruby when bash starts ##
To have a Ruby version shell selected and running when bash starts:

~~~ sh
$ echo 'rbenv local [version]' >> ~/.bashrc
~~~

Then restart the terminal. Check Ruby is running by typing `$ Ruby`

## Make sure permissions for gems are correct
rbenv seems to leave permissions on it's directory world-writable, which 
Ruby will constantly warn you about. To fix this use 

~~~ sh
$ chmod -R go-w ~/.rbenv
~~~

## Install Bundler
Install bundler using:

~~~ sh
gem install bundler
~~~

## Installing Cmock or Ceedling
You should now be ready to follow the very simple instructions for 
installing [CMock](https://github.com/ThrowTheSwitch/CMock/blob/master/README.md) 
or [Ceedling](https://github.com/ThrowTheSwitch/Ceedling/blob/master/README.md)

### CMock
~~~ sh
$ git clone --recursive https://github.com/throwtheswitch/cmock.git # clone CMock and all sub-repos
$ cd cmock
$ bundle install # Ensures you have all RubyGems needed
$ bundle exec rake # Run all CMock library tests
~~~

### Ceedling
~~~ sh
$ gem install ceedling
~~~
~~~
