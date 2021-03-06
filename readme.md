# Rtools Packages [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/r-windows/rtools-packages?branch=master)](https://ci.appveyor.com/project/jeroen/rtools-packages)

A repository of pacman packages for use with the new experimental [rtools 4.0](https://cloud.r-project.org/bin/windows/testing/rtools40.html). Builds from the master branch are automatically deployed on [bintray](https://dl.bintray.com/rtools/) and become directly available in the `pacman` tool in Rtools 4.0.

## Enable Rtools40 to build libs

By default, the rtools40 build environment is frozen in a stable state for building R packages. If you want to build C/C++ libs, you first need to enable the (unstable) upstream msys2 repos. This can actually interfere with your rtools40 installation, so don't do this on important servers used to build R packages.

To enable the upstream repo, open the file `c:\rtools40\etc\pacman.conf` in a text editor and uncomment the following 2 lines at the very end of the file:

```
## 3rd party msys2 packages (rtools hackers only!)
[msys]
Include = /etc/pacman.d/mirrorlist.msys
```

Then open the rtools40 shell and run `pacman -Syu`. Then close the shell and restart the rtools40 shell.

Now the upstream msys repo repo is enabled, you can install to all extra tools needed to build libraries and other software. For example you can install autoconf or vim and so on:

```
pacman -S autoconf automake libtool
pacman -S vim
```

If you messed up the installation, I recommend you uninstall rtools40 and start over :-)


## Building Package

To build a mingw-w64 package, `cd` into the directory and run `makepkg-mingw` like this:

```
git clone https://github.com/r-windows/rtools-packages
cd mingw-w64-sqlite3
makepkg-mingw
```

To add or update a package, please send a pull request to this repo. Please make sure you have tested it locally before sending the pull request. 

If you want to add a new library, you can often adapt them from the upstream [msys2/MINGW-packages](https://github.com/msys2/MINGW-packages) repo. Please adapt the `PKGBUILD` to __only build static libraries__ (`libfoo.a` files): R packages need to be standalone, so __libs in the `rtools-packages` repo may not contain dll files__. This is important.
