## Printing on Manjaro

 Common Unix Printing System is free open source printing system used in most Linux distributions.

1. **Install the Printer Software**

   ```sh
   sudo pacman -S manjaro-printer
   ```

2. **Add yourself to the `sys` group**

   ```sh
   sudo gpasswd -a USERNAME sys
   # replace by real user name
   ```

3. **Enabling Printing Capabilities**

```sh
sudo systemctl enable --now cups.service
sudo systemctl enable --now cups.socket
sudo systemctl enable --now cups.path
```



At this point you should be ready to configure printer. Please  reboot PC after enabling printer. Some times printer can't be recognised.



If you have any further question please visit manjaro  [Manjaro Printing](https://wiki.manjaro.org/index.php/Printing)

