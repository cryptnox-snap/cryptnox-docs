
# Welcome to Cryptnox
## Cryptnox Snap 

## Snap - Build From Source

Snap is support multi arch and multi distro - we maintain **_64bit_** fully is **arm64** and **amd64**, However, **_32bit_** legacy is available  **armhf** and **i386**.

Here is many type of build environment

- multipass
- lxd
- docker
- remote-host
- native

```
git clone https://github.com/kokoye2007/cryptnox-snap
cd cryptnox-snap
snapcraft
```

depend on your build-environment, remote-host is build on launchpad.net and support by [Canonical](https://www.canonical.com) 

- https://github.com/kokoye2007/cryptnox-snap
- https://launchpad.net
- https://canonical.com

## Core - Build From Source

Snap Core is available 
 
 **Version**
 
- 16 (Ubuntu 16.04)
- 18 (Ubuntu 18.04)
- 20 (Ubuntu 20.04)
- 22 (Ubuntu 22.04)

**Arch**

 - arm64 
 - armhf 
 - i386 
 - ppc64el 
 - s390x 
 - amd64 
 - arm64 
 - armhf 
 - i386 
 - s390x

We are focus on Core18 for Testing and Core20 as a Stable Release Version.

Mainly target is arm64 on Raspberry Pi 4B

#### Additional / Available 
- PC 
	- amd64 
	- i386
- ARM
	- armhf 
	- arm64
	
### Build

```
git clone https://github.com/snapcore/core20
cd core20
# Edit for Customize / System Config / TPM and Modules
# Test
spread qemu-nested
# Build
snapcraft 
```

	
## [Cryptnox-Gadget](https://github.com/kokoye2007/cryptnox-gadget)

**NOTE** gadget is still need to update.
- ace ACR / pn533_usb
- pn532 board 
	- pn532 SPI
	- pn532 I2c 
	- pn532 All in One
- All in One Gadget 


If you are using manual install snap ? you need to do 

- manual hardware connect [once]
- manual hardware blacklist [once]
- manual services restart [when new card-reader attached, reboot or restart services]

for the raspberry pi - core images ?
we can do rather than 

- auto connect 
- auto hardware blacklist 
- auto start after boot 
- auto encrypt 
- cryptnox boot logo
- secure storage and encrypt is default 
and future purpose
- factory reset system

```
git clone https://github.com/kokoye2007/cryptnox-gadget
git checkout 20arm64 # or 18arm64
snapcraft clean --destructive-mode
snapcraft snap --target-arch=arm64 --destructive-mode
```

_**It is only for Raspberry Pi**_


# Build Core20 + Gadget

We are make / organized for **Core20**, **Gadget** and **Cryptnox Snap** and build for custom images.

Check list
-
- You must be already build _**pi-kernel**_ and _**gadget**_, otherwise use snap channel
- you must be already copy or build _**cryptnox snap**_ images.
- before test with _**spread**_
- Must be work, manual test in before build the Core Customize Images.

Build
-
- clone the repo
- build 
	- kernel image
	- gadget image
	- cryptnox snap image

Combine
-


#### Sample Build: 
```
cat model.json | snap sign -k default > model.model
ubuntu-image snap -O cryptnox20 model.model  --snap ../pi-gadget/cryptnox-pg_20-1_arm64.snap
```
#### example model:
```
{
    "type": "model",
    "series": "16",
    "authority-id": "EDFnw3whZEgAVWRBzyrQR9LUvK9pInoV",
    "brand-id": "EDFnw3whZEgAVWRBzyrQR9LUvK9pInoV",
    "system-user-authority": "*",
    "model": "cryptnox-core20",
    "architecture": "arm64",
    "timestamp": "%TIMESTAMP%",
    "base": "core20",
    "grade": "%GRADE%",
    "snaps": [
        {
            "default-channel": "20/stable",
            "id": "YbGa9O3dAXl88YLI6Y1bGG74pwBxZyKg",
            "name": "pi",
            "type": "gadget"
        },
        {
            "default-channel": "20/stable",
            "id": "jeIuP6tfFrvAdic8DMWqHmoaoukAPNbJ",
            "name": "pi-kernel",
            "type": "kernel"
        },
        {
            "name": "core20",
            "type": "base",
            "default-channel": "latest/stable",
            "id": "DLqre5XGLbDqg9jPtiAhRRjDuPVa5X1q"
        },
         {
            "name": "snapd",
            "type": "snapd",
            "default-channel": "latest/stable",
            "id": "PMrrV4ml8uWuEUDBT8dSGnKUYbevVhc4"
        },
        {
            "name": "cryptnox",
            "type": "app",
            "default-channel": "latest/stable",
            "id": "M3WW8C4m1SMo3celf9LfeqIiaybskgZO"
        }
    ]
}

```
#### Build Script
```
#!/bin/bash

# stop execution if any commands in this script fail
set -e


# Create the signed model assertation. 
echo Creating Model Assertation

# For convenience, we replace %TIMESTAMP% with the current time
TIMESTAMP=`date -Iseconds --utc`
GRADE=dangerous
# GRADE=signed
# GRADE=secured

# #storage-safety
# SSAFETY=prefer-encrypted
# SSAFETY=encrypted
# SSAFETY=prefer-unencrypted

# Where craig-snapcraft-production is the name of the snapcraft key you've created 
cat model.json | sed -e "s/%TIMESTAMP%/$TIMESTAMP/g; s/%GRADE%/$GRADE/g" | snap sign -k default > model.model



# Build the image
echo Building Image

# pass cloud.conf to ubuntu image
# NOTE: if there are other snaps you are developing locally, then you can include them under required snaps in model.json and make ubuntu-image aware of them with ```--extra-snaps ./snap-name/snap-name_0.1_amd64.snap```
ubuntu-image snap -O Cryptnox20 model.model
# --snap ../pi-gadget/cryptnox-pg_20-1_arm64.snap

#       
#       # If you are running this on Ubuntu desktop, then it's very convenient to test the image right away in KVM
#       # You can SSH to the image via ssh craig@localhost -p 8022
#       if [ -f "/usr/bin/kvm" ]; then
#         echo Running Image
#         sudo kvm -smp 2 -m 1500 -netdev user,id=mynet0,hostfwd=tcp::8022-:22,hostfwd=tcp::8090-:80 -device virtio-net-pci,netdev=mynet0 -vga qxl -drive file=image/pc.img,format=raw
#       fi

```

### Snap Image Build with Snapcraft Store

Build with Local 

- Use LXD 
``` 
snapcraft --use-lxd
```
- Use Multiplass - Default 
```
snapcraft
```
- Use Launchpad 
```
snapcraft remote-build
```
- Use Native with RPi 4
```
# RPi4
snap install lxd
sudo lxd init --auto
sudo lxc launch ubuntu:20.04 focal
sudo lxc shell focal
...
# RPi4 LXC
snap install snapcraft --classic
git clone $CRYPTNOX-GIT-URL  $CRYPTNOX-GIT
cd $CRYPTNOX-GIT
snapcraft --destructive-mode
Snapped .......

......

cp cryptnox*.snap /tmp/
exit ....

# RPi4 
sudo lxc file pull focal/tmp/$SNAP_IMAGE.snap ./
sudo snap install --dangerous $SNAP_IMAGE.snap

# TEST
cryptnox tab tab
..
```

After testing 
- Manual Upload to Snap Store.

```
snapcraft login
```
```
RELEASE="stable|candidate|beta|edge"
SNAP=cryptnox-version.snap
snapcraft upload --release=$RELEASE $SNAP
```
ref: 
- https://snapcraft.io/docs/releasing-your-app 
- https://snapcraft.io/docs/release-management
- https://snapcraft.io/docs/using-the-snap-store

Or 

**Auto Upload with Github and Snapcraft**

Test on your RPi and Local 

- push to github
- connect with Snapcraft Store
- trigger from Snapcraft Build
- release from Snapcraft Store

![](https://assets.ubuntu.com/v1/ed6d1c5b-build.snapcraft.hero.svg)


Ref: 
- https://snapcraft.io/build

