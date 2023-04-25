# 仓库功能

提供补丁，为TN3399V3添加OpenWRT支持

# 构建OpenWRT镜像

## 准备环境

准备Ubuntu或Debian PC一台

安装以下依赖：

```
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pyelftools \
libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip \
vim wget xmlto xxd zlib1g-dev
```

## 打补丁

在OpenWRT github的[release界面](https://github.com/openwrt/openwrt/releases)下载源码并解压

将本仓库提供的补丁复制到源码根目录

在仓库源码根目录打开终端，执行patch -p1 < openwrt-22.03.4-add-tn3399v3-support.patch

## 编译

```
# 更新周边软件源
./scripts/feeds update -a
# 安装周边软件源
./scripts/feeds install -a
# 进入编译配置界面进行配置
make menuconfig
# 根据配置提前下载需要的软件源码，如果跳过这步直接进行下一步，会变成一边下载一边编译
make download V=s -j32
# 编译
make V=s -j32
```

# 构建注意事项

- 配置确保目标镜像的kernel image分区和rootfs分区有足够的空间来装载二进制文件

- 启用WLAN需要打开以下配置选项：

```
# 驱动支持
Kernel modules
    kmod-brcmfmac
        Enable SDIO bus interface support
# 固件支持
Firmware
    brcmfmac-firmware-43455-sdio-tn3399v3
```

仓库中有我的配置，复制到源码根目录重命名为`.config`即可使用，不一定适用所有人

- 可根据此[仓库](https://github.com/kenzok8/openwrt-packages)添加一些额外的常用软件包再编译
