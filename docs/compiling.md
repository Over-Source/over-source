## Compiling
* [Windows](#windows)
* [Linux, *BSD](#linux,-*bsd)

### Windows

**TODO**

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
