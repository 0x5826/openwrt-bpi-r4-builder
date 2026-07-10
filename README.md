# OpenWrt Mediatek BPi-R4 固件自动化编译系统

基于 GitHub Actions 的 **Banana Pi BPi-R4** 固件自动编译系统。

## 🌟 核心特性

- **源码与配置解耦**：此控制仓库仅存放编译配置与工作流，不污染 OpenWrt 主源码仓库。
- **定时与手动触发**：
  - **定时编译**：每周五下午 18:00 (北京时间 / 10:00 UTC) 自动拉取编译。
  - **手动编译**：支持在 GitHub **Actions** 页面手动触发（`workflow_dispatch`）。
- **自动化发布 Release**：编译成功后，会自动创建以编译时间戳命名的 GitHub Release 并上传所有的构建固件资产。

## 📂 固件制品说明

编译生成的固件包括：
- `*-sdcard.img.gz`：SD 卡启动系统镜像。
- `*preloader.bin / *uboot.fip`：SPI NAND / eMMC 引导程序。
- `*-emmc-gpt.bin`：eMMC GPT 分区表文件。
- `*-squashfs-sysupgrade.itb`：Squashfs 格式系统升级镜像。
- `*.manifest` / `*.buildinfo`：软件包清单及编译元数据。
