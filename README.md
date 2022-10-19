# Setup

lsusb

```shell
[fahmad@ryzen ~]$  lsusb
...
Bus 005 Device 005: ID 2e8a:0003 Raspberry Pi RP2 Boot
...
```

## pico-sdk

```shell
mkdir pico
cd pico
git clone -b master https://github.com/raspberrypi/pico-sdk.git
cd pico-sdk
git submodule update --init
cd ..
git clone -b master https://github.com/raspberrypi/pico-examples.git

```

if you list the pico folder now looks like this

```shell
[fahmad@ryzen pico]$  ls
pico-examples  pico-sdk
```

## install packages

```shell
sudo dnf install gcc-arm-linux-gnu arm-none-eabi-newlib arm-none-eabi-gcc-cs arm-none-eabi-gcc-cs-c++
```

## blink onboard LED

```shell
cd ~/pico/pico-examples/
mkdir build
cd build
export PICO_SDK_PATH=../../pico-sdk
```

now, since I have pico w board, I need to specify the board

```shell
cmake -DPICO_BOARD=pico_w ..
```

go to the blink folder for `pico_w`

```shell
cd pico-examples/build/pico_w/blink
make -j4
```

## upload the `.uf2`

hold down the `BOOTSEL` on the pico w as you plug it in. mount the pico to `/mnt`

```shell
sudo mkdir -p /mnt/pico
sudo mount /dev/sdd1 /mnt/pico
```

if you list the `/mnt/pico/` folder, you'll see two files already there

```shell
[fahmad@ryzen blink]$  ls /mnt/pico
INDEX.HTM  INFO_UF2.TXT
```

copy the `.uf2` file to `/mnt/pico/`, then unmount the pico w.

```shell
[fahmad@ryzen blink]$ pwd
/home/fahmad/pico/pico-examples/build/pico_w/blink
[fahmad@ryzen blink]$  ls
CMakeFiles           picow_blink.bin  picow_blink.elf.map
cmake_install.cmake  picow_blink.dis  picow_blink.hex
Makefile             picow_blink.elf  picow_blink.uf2
[fahmad@ryzen blink]$  sudo cp picow_blink.uf2 /mnt/pico/
[fahmad@ryzen blink]$  sudo umount /mnt/pico
```

![pico w blink](./images/pico-w-blink.gif)
