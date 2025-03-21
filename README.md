---
title: Portable Fuchsia Emulator (FImage)
permalink: os/fimage.html
summary: Explanation of the FImage tool, and its usage
---
## Portable Fuchsia Emulator (FImage)
### Note
FImage is provided for the convenience of developers and enthusiasts who don't want to compile Fuchsia.
We are in no way affiliated Google. For more information, see [fimage/LICENSE](https://github.com/dahliaOS/fimage/blob/master/LICENSE).                 
The windows method is highly unstable and should only be used if you cannot install or boot into linux on your device.

### TL;DR
* The FImage emulator can be downloaded [here](https://github.com/dahliaOS/fimage/releases).
* For more information on the Fuchsia Emulator, see [this document](https://fuchsia.dev/fuchsia-src/concepts/emulator).

To quickly run FImage, use the commands below inside of its folder:
```bash
./ffx-linux-x64 platform preflight
./network-config
./fimage-gui 4096
```

See [Explore Fuchsia](https://fuchsia.dev/fuchsia-src/get-started/explore_fuchsia) for tips on what you can do next.
When you're done, you can clean up via `dm shutdown`.

### Recommended Requirements
* linux
    * 8GiB of RAM or more
    * an Intel processor produced after 2010 (If you have a dedicated GPU)
    * A 4th generation Intel processor (If you do not have a dedicated GPU) (Ivy Bridge *technically* works, but has all sorts of visual bugs)
    * Ubuntu 20.04 or equivalent
    * curl, unzip,git
    * Up-to-date graphics drivers

* windows (wsl2)
    * Windows 10/11 build that supports wsl2 with wslg.
    * Ubuntu 20.04 wsl2 with wslg
    * 16GiB of RAM or more

# linux

### Fimage Quick Start
First, download the latest Fimage tool at [fimage/releases](https://github.com/dahliaOS/fimage/releases).
Extract the file `fimage-<version>.zip` and go into the fimage folder. 

Begin by checking your hardware, using the provided `ffx` tool. [FFX Documentation on fuchsia.dev](https://fuchsia.dev/fuchsia-src/reference/tools/sdk/ffx)
```bash
./ffx-linux-x64 platform preflight
```
This will print information about the hardware and software. If you are missing any dependencies or lacking hardware, it will let you know. The most common error is related to a lack of a supported GPU, to negate this, FImage uses software rendering by default, which may affect performance. If you have a supported GPU (Intel Ivy Bridge or newer), use the `fimage-gui-hostGPU` script to run FImage.

After following the instructions generated by the ffx preflight checks, you will need to configure networking, using the command below. This will configure the network interfaces for FEMU.
```bash
./network-config
```
Finally, the emulator is ready to run! Select one of the different launch options and use that to launch the emulator.

The command syntax is the same for each option. For example, to launch an FImage instance with 4GiB of RAM and a GUI, use:
```bash
./fimage-gui 4096
```
The launch options are as follows:
* fimage-headless - Fuchsia emulator using only the command line
* fimage-gui - Fuchsia emulator with the FEMU interface and the Ermine user shell
* fimage-gui-hostGPU - Same as fimage-gui, using hostGPU; if supported

### FImage on dahliaOS Linux-based Builds

Fimage can be installed on dahliaOS Linux with:
```bash
dap install fimage
```

### Flutter Development with FImage
A guide to Flutter development with FImage can be found here: [Setting up FImage for Application Development](/developer/fimage-setup)


# Windows (wsl2)

A **WIP** script is in testing thats should enable to atleast boot fuchsia in the terminal. Booting fuchsia with the fimage gui is possible but highly unstable.

### Fimage Quick Start

First install wsl2 on windows by opening **powershell** or **windows terminal** and running this command.
```bash
wsl --install
```

After that reboot your machine and open up again  **powershell** or **windows terminal** and running the first command to use fimage in cli mode **recommended** or the second command to use fimage in gui mode. If you use the gui mode and close fimage make sure you run the reset command afterwards.

**cli command**
```bash
wget https://docs.dahliaos.io/scripts/fimage/windows/wsl2/launch.sh && sudo chmod +x launch.sh; sudo ./launch.sh -cli
```

**gui command**
```bash
wget https://docs.dahliaos.io/scripts/fimage/windows/wsl2/launch.sh && sudo chmod +x launch.sh; sudo ./launch.sh -gui
```

**reset command**
```bash
wget https://docs.dahliaos.io/scripts/fimage/windows/wsl2/launch.sh && sudo chmod +x launch.sh; sudo ./launch.sh -reset
```

Command-line interface in gnome-terminal
### Known Issues
* Flutter development doesn't work yet, due to [a bug in the Flutter tool that escapes IPv6 addresses improperly](https://bugs.fuchsia.dev/p/fuchsia/issues/detail?id=77566)
* Performance when drawn with the software GPU is expectedly awful
* Mouse input is laggy
* Terminal application within ermine crashes (Fuchsia bug?); negated by pressing enter in the terminal FEMU was launched from.
* Extreme jank on Ivy Bridge devices using host GPU

### compiling Fuchsia for Fimage
Follow the documented steps from fuchsia.dev and then set the full target with tools, `fx set workstation.qemu-x64 --with-base=//bundles:tools`

export FUCHSIA_SSH_CONFIG="/Users/nmcain/fuchsia/out/default/ssh-keys/ssh_config"
