# EBAZ4205

## Description

This repository contains Vivado and AMD EDF (formerly PetaLinux) projects for the Zynq EBAZ4205 board.  It is based on the EBAZ4205 git repository by KeitetsuWorks, updated for v2025.2 of the AMD tools and with fixes for the operation of the tactile switches S2 and S3.  The default build of Linux in this repository includes the package management tools so that additional components may be installed with the dnf command after booting.

Branches '202n.n' contain files for v202n.n of Vivado/PetaLinux.  Branch 'main' contains files for v2025.2 of Vivado/AMD EDF.

## Requirement

### Hardware

* Zynq EBAZ4205 Board (cost-reduced version)
  * **No** 25MHz crystal (Y3) is required. The Ethernet transceiver (U24) clock is supplied by the ZYNQ (U31). However, it also works on a board on which a crystal is mounted
  * microSD card slot (U7) required
  * SD card boot support is required. Short the resistor (R2577)
  * Short the diode (D24) to supply power from the power connector (J4) (Optional)
  * Mount the tactile switch (S3), the capacitor (C2410) and the resistor (R2649) (Optional).

### Software

* AMD Vivado 2025.2
* Git repo tool [https://gerrit.googlesource.com/git-repo](https://gerrit.googlesource.com/git-repo)

## How to Build 

* [How to Build](./docs/how-to-build.md)

## References

### EBAZ4205

* [KeitetsuWorks/EBAZ4205: Vivado and PetaLinux (v2021.2) projects for the Zynq EBAZ4205 Board.](https://github.com/KeitetsuWorks/EBAZ4205)
* [xjtuecho/EBAZ4205: A 5$ Xilinx ZYNQ development board.](https://github.com/xjtuecho/EBAZ4205)
  * First setup
  * Schematics
  * Xilinx Design Constraints
  * mtd information
* [Leungfung/ebaz4205_hw](https://github.com/Leungfung/ebaz4205_hw)
  * Document (Block desgin)
* [kan573 - Qiita](https://qiita.com/kan573)
  * Articles in Japanese
* [blkf2016/ebaz4205: Some resources for the ebaz4205 board](https://github.com/blkf2016/ebaz4205)
  * Sample project
* [nightseas/ebit_z7010: The base reference design for EBIT EBAZ4205 Zynq7010 board.](https://github.com/nightseas/ebit_z7010)
  * Sample project

### AMD

* [UG585 - Zynq-7000 SoC Technical Reference Manual (v1.14)](https://docs.amd.com/r/en-US/ug585-zynq-7000-SoC-TRM)
* [UG865 - Zynq-7000 SoC Packaging and Pinout Product Specification (v1.9)](https://docs.amd.com/v/u/en-US/ug865-Zynq-7000-Pkg-Pinout)
* [XC7Z010CLG400 Pinout](https://download.amd.com/adaptive-socs-and-fpgas/developer/adaptive-socs-and-fpgas/package-pinout-files/z7packages/xc7z010clg400pkg.txt)
* [AMD Embedded Development Framework (EDF) - AMD Adaptive Computing Wiki - Confluence](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/3250585601/AMD+Embedded+Development+Framework+EDF)
* [UG1144 - PetaLinux Tools Documentation Reference Guide (v2024.2)](https://docs.amd.com/r/en-US/ug1144-petalinux-tools-reference-guide)
* [LEDS-GPIO Driver - Linux GPIO Driver - AMD Adaptive Computing Wiki - Confluence](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842398/Linux+GPIO+Driver#LinuxGPIODriver-LEDS-GPIODriver)
* [GPIO-Keys Driver - Linux GPIO Driver - AMD Adaptive Computing Wiki - Confluence](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842398/Linux+GPIO+Driver#LinuxGPIODriver-GPIO-KeysDriver)

### Linux Kernel

* [LEDs connected to GPIO lines](https://www.kernel.org/doc/Documentation/devicetree/bindings/leds/leds-gpio.txt)
* [Common leds properties](https://www.kernel.org/doc/Documentation/devicetree/bindings/leds/common.yaml)
* [Device-Tree bindings for input/keyboard/gpio_keys.c keyboard driver](https://www.kernel.org/doc/Documentation/devicetree/bindings/input/gpio-keys.txt)

### Others

* [Vivadoでプロジェクトのエクスポートを極める - Qiita](https://qiita.com/nahitafu/items/de4b295ea60ce6173a83)
* [MII通信　～MACとPHYをつなぐインターフェース～ - 半導体事業 - マクニカ](https://www.macnica.co.jp/business/semiconductor/articles/microchip/134946/)
* [ARM PrimeCell Static Memory Controller (PL350 series) Revision: r2p1 Technical Reference Manual](https://developer.arm.com/documentation/ddi0380/g/?lang=en)
* [Winbond W29N01HVxINA Datasheet](https://www.winbond.com/resource-files/w29n01hvxina_revc.pdf)