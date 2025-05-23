# Brew Kernel

<img src="asciiart.png" width="200" /> </br>
Brew Kernel is a simple x86_64 kernel that demonstrates basic OS concepts. It features a custom bootloader, VGA text mode output with customizable colors, and basic interrupt handling.

<img src="img/Screenshot 2025-04-28 at 00.23.37.png" width="500" alt="QEMU Screenshot" /><br />
<sub><i>Note: This screenshot may be outdated.</i></sub>
## Features

- 64-bit long mode support
- Multiboot2 compliant
- Custom VGA text mode driver with 16-color palette support
- Basic Interrupt Descriptor Table (IDT) implementation
- ASCII character display demo
- Integer output support with signed and unsigned number handling
- Ability to run on actual x86_64 hardware


## Prerequisites

To build the kernel, you'll need Docker installed on your system. The build environment is containerized to ensure consistency across different systems.

## Building

1. First, build the Docker container for the build environment:

```sh
cd BrewKernel
docker build buildenv -t brewkernel    
```

2. Run the build script:

```sh
docker run --rm -it -v "$(pwd)":/root/env -w /root/env --platform linux/amd64 brewkernel make build-x86_64    
```

The build process will create:
- A kernel binary at `dist/x86_64/kernel.bin`
- A bootable ISO image at `dist/x86_64/kernel.iso`

## Running

You can run the kernel using QEMU:

```sh
qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
```

Or run Brew on actual hardware. </br>
<img src="img/Screenshot 2025-04-28 at 00.23.05.png" width="400" /> </br>


*note: This is not recommended, i am not liable if this software breaks your system, this is **completely at YOUR OWN RISK.** This software comes with **ZERO** warranty.*

- Step 1:
    Install BalenaEtcher on your device </br>
    *From own testing this should work, if you would like to use DD, then do so. At again, your own risk.*

- Step 2:
    Open BalenaEtcher and choose the kernel.iso, an external USB drive and flash it, this should take a second, if not less.
- Step 3:
    Find a compatible x86_64 system.
    ```
    Tested Hardware:
    HP EliteDesk 705 G4 DM 65W SBKPF 
     CPU:   AMD Ryzen 5 PRO 2400G (8) @ 3.600GHz
     GPU:   AMD ATI Radeon Vega Series (VGA ONLY.)
    ```
- Step 4:
    Enable legacy boot in your BIOS
- Step 5:
    Insert the flashed USB Drive and boot using the legacy boot option **NOT UEFI**. </br>
    **Important Notice: You can only get a display output by using a VGA port for display output, anything other than VGA is NOT supported.**
- Step 6:
    Enjoy the Brew Kernel!


## Project Structure

- `src/impl/kernel/` - Main kernel implementation
- `src/impl/x86_64/` - Architecture-specific code
- `src/intf/` - Header files and interfaces
- `targets/` - Target-specific files (linker scripts, GRUB config)
- `buildenv/` - Docker build environment
- `dist/` - Build output directory

## Technical Details

### Memory Map

- Kernel is loaded at 1MB (0x100000)
- Stack is 16KB
- Page tables are set up for identity mapping

### VGA Text Mode

- Resolution: 80x25 characters
- 16-color palette support
- Memory mapped at 0xB8000

### Integer Output Support

- Signed integer printing with negative number handling
- Unsigned integer printing for positive numbers only
- Base-10 (decimal) output format
- Automatic buffer management for any integer size
- Numbers automatically wrapped at screen boundaries

### Interrupts

Basic IDT setup with handlers for:
- Division by Zero (Vector 0)
- Debug Exception (Vector 1)
- Page Fault (Vector 14)


###
###

<h2 align="left">Help me brew some coffee! ☕️</h2>

###

<p align="left">
  If you enjoy this project, and like what i'm doing here, consider buying me a coffee!
  <br><br>
  <a href="https://buymeacoffee.com/boreddevhq" target="_blank">
    <img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" height="50" style="border-radius: 8px;" />
  </a>
</p>

###


## License

Copyright (C) 2024-2025 boreddevhq

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
