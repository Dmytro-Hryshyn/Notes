## Arduino



## Getting started with Arduino on Linux

To be able to upload sketches to Arduino board user should be in special group.  Users may need to add own “username” to the `dialout` group if they are not “root”

### Ubuntu/Debian

```sh
sudo usermod -a -G dialout $USER
sudo usermod -a -G plugdev $USER
```



### Arch, Manjaro

```sh
sudo usermod -a -G uucp $USER
sudo usermod -a -G lock $USER
```

After adding to special group restart PC

