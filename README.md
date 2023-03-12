# void-on-macbookair62
# Void Linux Macbook Air 2013

## Base Install:

** $ Means user, # Means root/sudo **

```
$ cfdisk /dev/sda
```
* Delete all partitions and write

```
$ void-installer
```

Set up the following partitions:
```
/dev/sda1 512M EFI
/dev/sda2 4G   swap
/dev/sda3 --   linux
```

When choosing filesystems, order matters!
```
/dev/sda3 ext4  /
/dev/sda2 swap
/dev/sda1 vfat  /boot/efi
```

## Post Install:

### date

```
$  xbps-install ntp dracut
```

 ```
 $ ntpd
 ```
 ** Then check time
 ```
 $ date
 ```
 
 ```
 $ hwclock --systohc
 ```

### networking
```
$ git clone git://github.com/void-linux/void-packages
$ cd void-packages
$ ./xbps-src binary-bootstrap
$ ./xbps-src pkg broadcom-wl-dkms
$ xbps-install --repository=hostdir/binpkgs/nonfree broadcom-wl-dkms
$ xbps-install -S dbus && ln -s /etc/sv/dbus /var/service
$ wpa_passphrase <ssid> <key> >> /etc/wpa_supplicant/wpa_supplicant.conf
```
  
### sound
```
$ xbps-install alsa-utils
```

Choose soundcard with F6 and unmute with 'm'. Add to /etc/asound.conf:
```
defaults.pcm.card 1
defaults.pcm.device 0
defaults.ctl.card 1
```

### trackpad
Run:
```
$ echo "add_drivers+=\"bcm5974\"" > /etc/dracut.conf.d/10-touchpad.conf
$ dracut --force
```

### other stuff
Fans:
```
$ xbps-install mbpfan
$ ln -s /etc/sv/mbpfan /var/service
$ mbpfan -t
```
 
Lid hibernate:
```
$ ln -s /etc/sv/acpid /var/service
```
  
Powersave:
```
$ xbps-install thermald
$ ln -s /etc/sv/thermald /var/service
$ xbps-install powertop
$ powertop --auto-tune
```

Microcode updates:
```
$ xbps-install -S intel-ucode
$ xbps-reconfigure -f linux
```

Desktop env:
```
$ xbps-install xorg-minimal dejavu-font-ttf xf86-video-intel
```
   
## References
* https://wiki.voidlinux.org/Installation
* https://wiki.voidlinux.org/Installation:_UEFI
* https://wiki.voidlinux.org/Network_Configuration#WPA-PSK_encryption_.28WPA_Personal.29
* https://wiki.voidlinux.org/Macbook
* https://wiki.archlinux.org/index.php/Mac#Mid_2013_13"_-_Version_6,2
* https://www.ifnull.org/articles/void_mouse_on_mac/
