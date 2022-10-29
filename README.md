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
apt download dpkg-repack
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


## Credits

 - Debian Bug report logs - #892664 [dpkg: Please add decompression support for zstd](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=892664)