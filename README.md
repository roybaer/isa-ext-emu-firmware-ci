# Firmware CI for U-Boot and OpenSBI with ISA extension emulation

This repository provides automated CI builds for opensbi-isa-ext-emu (an OpenSBI fork with ISA extension emulation) with matching U-Boot for the following RISC-V boards:

- Starfive VisionFive 2
  - SoC: JH7110
  - ISA (native): rv64imafdc_zicntr_zicsr_zifencei_zihpm_zba_zbb
  - ISA (emulated): RVA22U64, RVB23U64
- Xunlong Orange Pi RV2
  - SoC: Ky X1 (a SpacemiT K1 derivative)
  - ISA (native): rv64imafdcv_zicbom_zicboz_zicntr_zicond_zicsr_zifencei_zihintpause_zihpm_zfh_zfhmin_zca_zcd_zba_zbb_zbc_zbs_zkt_zve32f_zve32x_zve64d_zve64f_zve64x_zvfh_zvfhmin_zvkt_sscofpmf_sstc_svinval_svnapot_svpbmt
  - ISA (emulated): RVA23U64

The generated binaries can be downloaded from the release page.
Please note that these nightly builds, as well as opensbi-isa-ext-emu itself, have to be considered experimental.
Their proper functioning cannot be guaranteed and they are not recommended for use in production environments.

Example boot log
![Picture](image.png)

## Installation / Flashing

See the official VisionFive 2 guide for information on how to flash the firmware on the VisionFive 2.
- https://doc-en.rvspace.org/VisionFive2/Quick_Start_Guide/VisionFive2_SDK_QSG/recovering_bootloader%20-%20vf2.html
- https://doc-en.rvspace.org/VisionFive2/Quick_Start_Guide/VisionFive2_SDK_QSG/spl_new.html#updating_spl_and_u_boot-vf2__section_zpj_cqt_yvb

For your convenience, these are the two most important lines:

~~~sh
flashcp -v u-boot-spl.bin.normal.out /dev/mtd0
flashcp -v visionfive2_fw_payload.img /dev/mtd2
~~~

For the Orange Pi RV2, look at the system's `/usr/lib/u-boot/platform_install.sh` and adapt the relevant `dd` lines, e.g. like this:

~~~sh
dd if=bootinfo_sd.bin of=/dev/mmcblk0 seek=0 conv=notrunc
dd if=FSBL.bin of=/dev/mmcblk0 seek=256 conv=notrunc
dd if=u-boot-env-default.bin of=/dev/mmcblk0 seek=768 conv=notrunc
dd if=u-boot-opensbi.itb of=/dev/mmcblk0 seek=1664 conv=notrunc
~~~

## Aknowledgements

This build automation for opensbi-isa-ext-emu has been adapted from previous work by Iris Shi, namely the visionfive2-firmware-ci.
