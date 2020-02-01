---
title: "ARM images"
linkTitle: "ARM images"
weight: 4
description: >
  Use Luet to build, track, and release OTA update for your embedded devices.
---


We will see an example on how to build burnable SD images for Raspberry PI with Luet. This approach let you describe and version OTA upgrades for your embedded devices, delivering upgrades as layer upgrades on the pi.

The other good side of the medal is that you can build a Luet package repository with multiple distributions (e.g. `Raspbian`, `OpenSUSE`, `Gentoo`, ... ) and switch among them in runtime. In the above example `Raspbian` and `Funtoo` (at the time of writing) are available.

## Prerequisites

You have to run the following steps inside an ARM board to produce arm-compatible binaries. Whatever distribution with Docker will work. Note that the same steps could be done in a cross-compilation approach, or either with qemu-binfmt in a amd64 host. 

## Build the packages

Clone the repository https://github.com/Luet-lab/luet-embedded