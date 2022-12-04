## Compiling
* [Windows](#windows)
* [Linux, *BSD](#linux-bsd)

### Windows

Untested

#### Requirements

* [CMake](https://cmake.org/download/)
* [Visual Studio 2022](https://visualstudio.microsoft.com/downloads/) and Multibyte MFC Library with a Windows SDK

#### Compiling

Open CMD, run:
```sh
cmake -S . -B build
```

Open the build folder and the visual studio solution. Right click on `ALL_BUILD` and build.

### Linux, *BSD

#### Requirements

The following is required:
| Distro | Dependencies |
|---|---|
| Arch Linux | `sudo pacman -S cmake sdl2 gcc lib32-glibc lib32-gcc-libs` |
| Debian / Ubuntu | `sudo apt-get install cmake libsdl2-dev build-essential gcc-multilib g++-multilib` |

#### Compiling

Open terminal in the source dir (sp/src or mp/src) and type:
```sh
cmake -S . -B build
cd build
make -f Makefile
```

On Linux, its possible to use all available CPU cores with a argument.

```sh
make -f Makefile -j$(nproc)
```
