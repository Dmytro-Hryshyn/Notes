## Pacman

The [pacman](https://archlinux.org/pacman/) [package manager](https://en.wikipedia.org/wiki/Package_manager) is one of the major distinguishing features of Arch Linux. It combines a simple binary package format with an easy-to-use [build system](https://wiki.archlinux.org/title/Arch_Build_System). The goal of *pacman* is to make it possible to easily manage packages, whether they are from the [official repositories](https://wiki.archlinux.org/title/Official_repositories) or the user's own builds.

Pacman keeps the system up to date by synchronising package lists with the master server. This server/client model also allows the user to download/install packages with a simple command, complete with all required dependencies.

Pacman is written in the [C](https://wiki.archlinux.org/title/C) programming language and uses the [bsdtar(1)](https://man.archlinux.org/man/bsdtar.1) [tar](https://en.wikipedia.org/wiki/tar_(computing)) format for packaging.



**To install a single package or list of packages**, including dependencies, issue the following command:

```sh
pacman -S package_name1 package_name2 ...
```

**Remove a package**, leaving all of its dependencies installed:

```sh
pacman -R package_name
```

**Remove a package and its dependencies** which are not required by any other installed package:

```sh
pacman -Rs package_name
```

**Update all packages**

```sh
pacman -Syu
```

**Search for package**

```sh
pacman -Ss string1 string2 ...
```

**Search for already installed packages**

```sh
pacman -Qs string1 string2 ...
```

**Search locally installed packages**

```sh
pacman -Qi package_name
```

**Download a package without installing it**

```sh
pacman -Sw package_name
```

**Install a local package** that is not from remote repository  [AUR](https://wiki.archlinux.org/title/AUR)

```sh
pacman -U /path/to/package/package_name-version.pkg.tar.zst
```

**Install a 'remote' package** (not from a repository stated in *pacman'*s configuration files):

```sh
pacman -U http://www.example.com/repo/example.pkg.tar.zst
```



For further information please visit [Arch Wiki](https://wiki.archlinux.org/title/pacman) 