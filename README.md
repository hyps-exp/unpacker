# unpacker

Unpacker is a C++ library for decoding raw data recorded with HDDAQ, which is used at J-PARC K1.8 beamline.

It reads HDDAQ raw data and converts it into
detector / plane / segment / channel data that analysis programs can use.

## Requirements
Unpacker is written in C++ and built with GNU make.
Several additional packages are also necessary for compiling.

| Software   | Version    | Note                                |
|------------|------------|-------------------------------------|
| GCC        | 8 or later | C++17 support is required           |
| GNU make   |            |                                     |
| zlib       |            | Reading `.gz` files                 |
| bzip2      |            | Reading `.bz2` files                |
| Xerces-C++ |            | Reading the XML configuration files |

The following libraries require **development packages** (names ending in `-devel` on RHEL, or `-dev` on Debian/Ubuntu).
- zlib
- bzip2
- Xerces-C++

### RHEL 9 / AlmaLinux 9
`xerces-c-devel` is provided by EPEL.
```sh
sudo dnf install epel-release
sudo dnf install gcc-c++ make zlib-devel bzip2-devel xerces-c-devel
```

### Debian / Ubuntu
```sh
sudo apt install g++ make zlib1g-dev libbz2-dev libxerces-c-dev
```

## Installation

### Build
```sh
git clone https://github.com/hyps-exp/unpacker.git
cd unpacker/src
cp Makefile.org Makefile
make
```

### Setup
Programs that use the unpacker find it with `unpacker-config`, so add the `bin` directory to your `PATH`.

Add the following line to `~/.bashrc`:
```sh
export PATH=/path/to/unpacker/bin:$PATH
```
Then reload it (or open a new terminal):
```sh
source ~/.bashrc
```

> [!WARNING]
> If you have more than one unpacker (for example, for different experiment),
> only the first `unpacker-config` found in `PATH` is used.
> Keep only one of them in `PATH` (check your `~/.bashrc`).
> Run `unpacker-config --prefix` to ensure the correct one is used.

Check the setup:
```sh
unpacker-config --prefix
```
It should print the directory where you cloned this unpacker
(e.g. `/home/user/hyps/unpacker`).

### Clean (optional)
Run following commands only when you need to rebuild from scratch.
```sh
cd src
make clean       # remove object files
make distclean   # also remove the contents of include/, lib/ and bin/
```

> [!NOTE]
> If you move or rename the unpacker directory, run `make distclean` and build again.
> The old path is embedded in the libraries, and programs may fail to find them
> depending on your environment.
> Programs that use the unpacker should also be rebuilt.
