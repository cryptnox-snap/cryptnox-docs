
# Welcome to Cryptnox

## User Guide

**A command line user interface to manage and use of Cryptnox cards.**

This provides basic wallets for Bitcoin and Ethereum. It is able to execute cleos commands and use the keys on the card for signing.

To buy NFC enabled cards that are supported by this library go to  [https://www.cryptnox.com/](https://www.cryptnox.com/) and  [snap](https://snapcraft.io/cryptnox)

-   please connect devices access
    
		snap connect cryptnox:hardware-observe
    
	    snap connect cryptnox:raw-usb

		snap connect cryptnox:i2c pi:i2c-1
    
-   please check
    
	    snap connections cryptnox
    
-   devmode
    
	    snap install cryptnox --devmode

  
-   services
    
	    snap services cryptnox
    
	    snap restart cryptnox.pcscd

- i2c

      sudo cryptnox.i2c-detect -y 1
      
      sudo cryptnox.nfc-list -v

      sudo cryptnox.nfc-scan -v

      sudo cryptnox.i2c-activate
    
-   test
    
	    cryptnox.pcsc-scan
    
	    cryptnox.card


# SNAP

Snap is Linux multiple distro support and easy to manage and install.

## Support Distro 

### Distributions with snap pre-installed
	
[KDE Neon](https://neon.kde.org/)

[Manjaro](https://manjaro.org/)

[Solus 3](https://getsol.us/home/)  and above

[Ubuntu 20.10](https://discourse.ubuntu.com/t/groovy-gorilla-release-notes/15533)  and  [Ubuntu 21.04](https://discourse.ubuntu.com/t/hirsute-hippo-release-notes/19221)

[Ubuntu 20.04 LTS (Focal Fossa)](https://releases.ubuntu.com/20.04/)

[Ubuntu 18.04 LTS (Bionic Beaver)](https://www.ubuntu.com/desktop/features)

Most  [Ubuntu flavours](https://wiki.ubuntu.com/DerivativeTeam/Derivatives)

[Zorin OS](https://zorinos.com/)

--- 
###  Distributions without snap pre-installed

[Arch Linux](https://snapcraft.io/docs/installing-snap-on-arch-linux)

[CentOS](https://snapcraft.io/docs/installing-snap-on-centos)

[Debian](https://snapcraft.io/docs/installing-snap-on-debian)

[elementary OS](https://snapcraft.io/docs/installing-snap-on-elementary-os)

[Fedora](https://snapcraft.io/docs/installing-snap-on-fedora)

[GalliumOS](https://snapcraft.io/docs/installing-snap-on-galliumos)

[Kali Linux](https://snapcraft.io/docs/installing-snap-on-kali)

[KDE Neon*](https://snapcraft.io/docs/installing-snap-on-kde-neon)

[Kubuntu](https://snapcraft.io/docs/installing-snap-on-kubuntu)

[Linux Mint](https://snapcraft.io/docs/installing-snap-on-linux-mint)

[Lubuntu](https://snapcraft.io/docs/installing-snap-on-lubuntu)

[Manjaro*](https://snapcraft.io/docs/installing-snap-on-manjaro-linux)

[openSUSE](https://snapcraft.io/docs/installing-snap-on-opensuse)

[Parrot Security OS](https://snapcraft.io/docs/installing-snap-on-parrot-security-os)

[Pop!_OS](https://snapcraft.io/docs/installing-snap-on-pop)

[Raspberry Pi OS](https://snapcraft.io/docs/installing-snap-on-raspbian)

[Red Hat Enterprise Linux (RHEL)](https://snapcraft.io/docs/installing-snap-on-red-hat)

[Rocky Linux](https://snapcraft.io/docs/installing-snap-on-rocky)

[Solus](https://snapcraft.io/docs/installing-snap-on-solus)

[Ubuntu*](https://snapcraft.io/docs/installing-snap-on-ubuntu)

[Xubuntu](https://snapcraft.io/docs/installing-snap-on-xubuntu)

[Zorin OS*](https://snapcraft.io/docs/installing-snap-on-zorin-os)


**Docs Available**

## Install Cryptnox Snap

``` snap install cryptnox ```

## Test Cryptnox on Snap

``` 
cryptnox.pcsc-scan 
```
Should detect card-reader and card-id

```
cryptnox.card
```
Should working cryptnox command line interface. 

### Cryptbox Snap Test and Deug

```
snap connections cryptnox
```
Output 
```
Interface         Plug                       Slot               Notes
content           -                          cryptnox:socket    -
hardware-observe  cryptnox:hardware-observe  :hardware-observe  manual
mount-observe     cryptnox:mount-observe     -                  -
network           cryptnox:network           :network           -
network-bind      cryptnox:network-bind      :network-bind      -
raw-input         cryptnox:raw-input         -                  -
raw-usb           cryptnox:raw-usb           :raw-usb           manual
removable-media   cryptnox:removable-media   -                  -
serial-port       cryptnox:serial-port       -                  -

```

```
snap services cryptnox
```
output 
```
Service         Startup  Current  Notes
cryptnox.pcscd  enabled  active   socket-activated
```
