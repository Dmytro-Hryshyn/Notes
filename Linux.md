## Linux terminal

### Network

| Command     | Action           |
| ----------- | :--------------- |
| hostname -i | Show IP  address |
| hostname    |                  |
|             |                  |



### Pacman

For full inforamation refer to [Arch wiki page](https://wiki.archlinux.org/title/Pacman)

Installing specific packages

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

