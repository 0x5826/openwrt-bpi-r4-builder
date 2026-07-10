# OpenWrt Mediatek BPi-R4 固件自动化编译系统

该仓库是专为 **Banana Pi BPi-R4** 设备定制开发的自动编译控制项目。采用 GitHub Actions 流水线，独立于源码仓库运行，以保持源码历史的整洁。

## 🌟 核心特性

- **源码与配置解耦**：此控制仓库仅存放编译配置与工作流，不污染 OpenWrt 主源码仓库。
- **定时与手动触发**：
  - **定时编译**：每周五下午 18:00 (北京时间 / 10:00 UTC) 自动拉取源码编译。
  - **手动编译**：支持在 GitHub **Actions** 页面手动一键触发（`workflow_dispatch`）。
- **自动化发布 Release**：编译成功后，会自动创建以北京时间编译时间戳命名的 GitHub Release（例如 `OpenWrt_BPi-R4_Build_202607101530`）并上传所有的构建固件资产。
- **鲁棒的 Feed 配置防御**：在编译更新 feeds 前，自动使用精准正则匹配检测 `feeds.conf.default`，防止重复写入自定义源（如 `nikki` 与 `dante_extras`）导致的编译冲突报错。

## 🛠 构建环境与编译依赖

- 构建运行环境：`ubuntu-22.04`。
- 集成了 `libelf-dev`、`python3-pyelftools`、`bc`、`time` 等 Mediatek 平台必需的核心编译依赖。
- 支持并发多线程编译，并在遇到竞态失败时自动回退为单线程 `V=s` 重新编译以输出详细错误日志。

## 📂 固件制品说明

编译生成的固件资产会自动分类并打包在 GitHub Release 和构建 Artifacts 中，主要包括：

- `*-sdcard.img.gz`：适用于 SD 卡启动的系统镜像文件。
- `*-snand-preloader.bin / *-snand-*-uboot.fip`：SPI NAND 的 preloader 与 U-Boot 引导程序。
- `*-emmc-preloader.bin / *-emmc-*-uboot.fip`：eMMC 的 preloader 与 U-Boot 引导程序。
- `*-emmc-gpt.bin`：eMMC GPT 分区表文件。
- `*-squashfs-sysupgrade.itb`：Squashfs 格式的系统升级镜像，用于系统后台升级。
- `*.manifest` / `*.buildinfo`：包含软件包版本清单及编译环境元数据。
