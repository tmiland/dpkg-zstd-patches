# debian-dpkg-zstd
 zstd decompression support for Debian

## This is a patched dpkg package for debian

 - This adds support for zstd decompression of ubuntu deb packages.
 - Why? If you would like to be able to install deb packages from a ubuntu repository in debian.

## Installation
- Download the deb packages and install:

```
git clone https://github.com/tmiland/dpkg-zstd-patches.git
```

```
sudo dpkg -i ./deb-packages/dpkg_1.20.12_amd64.deb
```
```
sudo dpkg -i ./deb-packages/dpkg-repack_1.47_all.deb
```

dpkg-repack is downloaded from the bullseye repo with:

```
apt download dpkg-repack=1.47
```
*** Not needed if you are running bullseye ***

On bookworm it's Because of the following error:
```
The following packages have unmet dependencies:
 dpkg-repack : Depends: dpkg (>= 1.21.0)
```

Add to `/etc/apt/preferences` to pin the dpkg packages:
 *** Keeps them from being updated ***

```
sudo nano /etc/apt/preferences
```

```
Package: dpkg dpkg-repack
Pin: release *
Pin-Priority: -100
```

## Testing

- Tested on Debian bookworm on Oct 29 2022

## Build dpkg from source

```
git clone https://git.dpkg.org/git/dpkg/dpkg.git
```

cd to root dpkg folder:

```
cd dpkg
```

```
git checkout 1.20.12
```

Apply patch:

Copy patch to root dpkg folder:

```
cp -rp ../patches/0001-dpkg-Add-Zstandard-compression-and-decompression-sup.patch .
```

```
patch -p1 -i 0001-dpkg-Add-Zstandard-compression-and-decompression-sup.patch
```

```
./autogen
```

```
./configure
```

```
make -j$(nproc)
```

```
make check
```

```
sudo dpkg-buildpackage -b -uc -us
```

### Adding Ubuntu archive alternative install (Debian bookworm)

Add to /etc/apt/source.list:

```
deb [signed-by=/usr/share/keyrings/archive.ubuntu.com-archive-keyring.gpg] http://no.archive.ubuntu.com/ubuntu kinetic main
```

Import the key:

```
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/archive.ubuntu.com-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 871920D1991BC93C
```

Add to /etc/apt/preferences:

```
Package: *
Pin: release a=kinetic
Pin-Priority: -1000
```

Run:

```
sudo apt update
```
Then:

```
sudo apt install dpkg=1.21.9ubuntu1
```

Replace the version with current. Check with:

```
apt policy dpkg
```

Remember to pin the dpkg package in /etc/apt/preferences

```
Package: dpkg
Pin: release *
Pin-Priority: -100
```

## Credits

 - Debian Bug report logs - #892664 [dpkg: Please add decompression support for zstd](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=892664)