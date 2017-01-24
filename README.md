# asdf-link

Generic plugin for versioning system tools with [asdf](https://github.com/asdf-vm/asdf).

## About

This is a plugin for tools that have *No* ASDF installer (maybe because the package
is hosted at a private location, or perhaps things that you already installed with
a vendor package manager, eg `brew` on OSX which supports many installed versions).

Or maybe things that can only be installed with some GUI interaction and for which 
creating an asdf installer would be a bit complicated, eg Android tools, AWS or Google Cloud tools.

For example, many systems already come with `perl` installed (two versions on my system).
Or on OSX where you can install the java tools via many methods, like the official package 
from [java.com](http://java.com), or by using other methods like `brew`, `macports`,
or sdk managers like [sdkman](http://sdkman.io/usage.html) or [jabba](https://github.com/shyiko/jabba),
there are lots of ways to get things installed on your system, and ASDF wont ever replace
them all.

In these cases you might still want to use the convenient ASDF `.tool-versions` file to
enable the right tool per project or system wide, so that you just cd into the
working directory of your like and let asdf select the right executable for you.

## Install

The first thing you have to do is to think of a good name. That is the name of the
tool you will be selecting versions for. Say `jdk`, `perl`, `android`, etc.

```shell
## READ above before copy-paste this line
$ asdf plugin-add NAME https://github.com/vic/asdf-link.git
```

This can be anything, from now on, these examples will be for `jdk`.

```shell
$ asdf plugin-add jdk https://github.com/vic/asdf-link.git
```

## Usage

Now if you execute `asdf list-all jdk` you will notice it will only say `system`.
That is because we cannot possibly know which versions are available. And actually,
this plugin will *let you install ANY version* you give to it. So it's up to you
to use a meaningful version. 

In my case, I have the following jdks `1.8` which I downloaded from the java
website and `1.9` which was installed with `brew install Caskroom/versions/java9-beta`

```shell
$ ls /Library/Java/JavaVirtualMachines/
jdk1.8.0_111.jdk jdk-9.jdk
```

To use them, lets tell ASDF about their existance with:

```shell
$ asdf install jdk 1.8
Link your system binaries to /Users/vic/.asdf/installs/jdk/1.8/bin

$ asdf install jdk 1.9
Link your system binaries to /Users/vic/.asdf/installs/jdk/1.9/bin
```

As previously mentioned, this plugin lets you install *any* version,
actually it just creates a `bin/` directory for you. The idea is that
we link (hence the plugin name) our versioned binaries into that `bin/` directory directory.

````shell
# linking all the java tools into the 1.9 versioned bin/
$ ln -vs /Library/Java/JavaVirtualMachines/jdk-9.jdk/Contents/Home/bin/* /Users/vic/.asdf/installs/jdk/1.9/bin/

# after this, just reshim
$ asdf reshim jdk
```

And we are done, you can create a `.tool-versions` in the current directory
by using `asdf local jdk 1.9`. See the [asdf](https://github.com/asdf-vm/asdf)
documentaion for more on managing versions.
