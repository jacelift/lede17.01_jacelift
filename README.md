created_at: 2022-01-28T09:37:41Z
=
```
https://api.github.com/repos/jacelift/lede17.01_jacelift
  "updated_at": "2022-01-28T09:39:01Z",
  "pushed_at": "2022-01-28T09:40:10Z",
```

【changelog】
=
```
【注意】：
kmod-fs-antfs与ntfs-3g冲突，二选一，否则编译失败
sfe与flowoffload是同一个，一个是旧版另一个是新版



【2022.8.13】
1.添加TL-AP300C-POE-V1机型到lede17.01_By_OMY-X1：
注意：固件的网口默认为LAN，当AP时：关闭dhcp服务即可；当路由时：互换LAN与WAN（含WAN6）的网络适配器eth1或eth0即可。
       尽量使用本机型的ART文件，否则容易出现各种bug。
grep -irl "OMY-X1" target/linux/*
grep -irl "OMY_X1" target/linux/*
TL-AP300C-POE-V1文件列表：12
target/linux/ar71xx/base-files/etc/diag.sh
target/linux/ar71xx/base-files/etc/board.d/01_leds
target/linux/ar71xx/base-files/lib/ar71xx.sh
target/linux/ar71xx/base-files/lib/upgrade/platform.sh
target/linux/ar71xx/image/generic.mk
target/linux/ar71xx/files/arch/mips/ath79/Kconfig.openwrt
target/linux/ar71xx/files/arch/mips/ath79/Makefile
target/linux/ar71xx/files/arch/mips/ath79/machtypes.h
target/linux/ar71xx/files/arch/mips/ath79/mach-tl-ap300c-poe-v1.c
target/linux/ar71xx/config-4.9
target/linux/ar71xx/config-4.14
target/linux/ar71xx/generic/config-default
==============================================

其中
修改：11
target/linux/ar71xx/base-files/etc/diag.sh
target/linux/ar71xx/base-files/etc/board.d/01_leds
target/linux/ar71xx/base-files/lib/ar71xx.sh
target/linux/ar71xx/base-files/lib/upgrade/platform.sh
target/linux/ar71xx/image/generic.mk
target/linux/ar71xx/files/arch/mips/ath79/Kconfig.openwrt
target/linux/ar71xx/files/arch/mips/ath79/Makefile
target/linux/ar71xx/files/arch/mips/ath79/machtypes.h
target/linux/ar71xx/config-4.9
target/linux/ar71xx/config-4.14
target/linux/ar71xx/generic/config-default
新增：1
target/linux/ar71xx/files/arch/mips/ath79/mach-tl-ap300c-poe-v1.c




【2022.7.17】
1.添加RX_WT600机型到lede17.01_By_newifi-d2：
RX_WT600文件列表：
target/linux/ramips/mt7621/base-files/etc/board.d/02_network
target/linux/ramips/mt7621/base-files/etc/board.d/01_leds
target/linux/ramips/dts/mt7621_RX_WT600.dts
target/linux/ramips/image/mt7621.mk
==========================================
修改:
target/linux/ramips/mt7621/base-files/etc/board.d/02_network
target/linux/ramips/mt7621/base-files/etc/board.d/01_leds
target/linux/ramips/image/mt7621.mk

新增:
target/linux/ramips/dts/mt7621_RX_WT600.dts
===========================================
修复：基于d2修改后只有5GHz无线网络，用E8822.dts（YouHua_WR1200JS.dts）文件的pcie0和pcie1部分替换mt7621_RX_WT600.dts的相应部分：
&pcie0 {
	mt76@0,0 {
		reg = <0x0000 0 0 0 0>;
		mediatek,mtd-eeprom = <&factory 0x0000>;
	};
};

&pcie1 {
	mt76@0,0 {
		reg = <0x0000 0 0 0 0>;
		mediatek,mtd-eeprom = <&factory 0x8000>;
		ieee80211-freq-limit = <5000000 6000000>;

		led {
			led-sources = <2>;
			led-active-low;
		};
	};
};
		




【2022.7.9】
1.添加ZTE_E8822机型到lede17.01_By_YouHua_WR1200JS：
ZTE_E8822文件列表：
target/linux/ramips/mt7621/base-files/etc/board.d/02_network
target/linux/ramips/mt7621/base-files/etc/board.d/01_leds
target/linux/ramips/dts/mt7621_ZTE_E8822.dts
target/linux/ramips/image/mt7621.mk
==========================================

修改:
target/linux/ramips/mt7621/base-files/etc/board.d/02_network
target/linux/ramips/mt7621/base-files/etc/board.d/01_leds
target/linux/ramips/image/mt7621.mk

新增:
target/linux/ramips/dts/mt7621_ZTE_E8822.dts
===========================================

2.修正LAN与WAN互反及用d2的修正LAN顺序：
target/linux/ramips/mt7621/base-files/etc/board.d/02_network
原始：
	ZTE,E8822)
		ucidef_add_switch "switch0" \
			"1:lan" "2:lan" "3:lan" "4:lan" "0:wan" "6@eth0"
		;;

修正LAN与WAN互反及用d2的修正LAN顺序：
	ZTE,E8822)
		ucidef_add_switch "switch0" \
			"0:lan:1" "1:lan:2" "2:lan:3" "3:lan:4" "4:wan:5" "6@eth0"
		;;


【2022.7.1】
1.解决tl-wdr5800-v1编译报错问题：丢失依赖库libcap.so.2，package/network/utils/iproute2/Makefile：
    1.1.Package/ip-full添加依赖+libcap
    1.2.Package/tc添加依赖+libcap
    注意:编译时make menconfig中选中libcap
2.修正tl-wdr5800-v1的LAN口顺序，target/linux/ar71xx/base-files/etc/board.d/02_network：
  tl-wdr5800-v1)
		ucidef_set_interfaces_lan_wan "eth1.1" "eth0"
		ucidef_add_switch "switch0" \
			"0@eth1" "1:lan:4" "2:lan:3" "3:lan:2" "4:lan:1"
		;;


【2022.2.11】
  添加tl-wr740n-v5机型支持（ar71xx-tiny-tl-wr740n-v5 16MB）:
  CPU：AR9331，RAM：64MB，ROM：16MB，2.4GHz：1X1 MIMO，面向端口布局：WLLLL。
一、参考tl-wr741nd-v4和tl-wr710n-v1添加740n-v5。
注意：依赖tl-wr710n-v1机型，确保tl-wr710n-v1机型的存在
（target/linux/ar71xx/image/tiny-tp-link.mk）
define Device/tl-wr740n-v4
  $(Device/tplink-16mlzma)
  DEVICE_TITLE := TP-LINK TL-WR740N/ND v4
  BOARDNAME := TL-WR741ND-v4
  DEVICE_PROFILE := TLWR740
  TPLINK_HWID := 0x07400004
  CONSOLE := ttyATH0,115200
endef
TARGET_DEVICES += tl-wr740n-v4

（target/linux/ar71xx/image/generic-tp-link.mk）
define Device/tl-wr710n-v1
  $(Device/tplink-16mlzma)
  DEVICE_TITLE := TP-LINK TL-WR710N v1
  DEVICE_PACKAGES := kmod-usb-core kmod-usb2
  BOARDNAME := TL-WR710N
  DEVICE_PROFILE := TLWR710
  TPLINK_HWID := 0x07100001
  CONSOLE := ttyATH0,115200
  IMAGE/factory.bin := append-rootfs | mktplinkfw factory -C US
endef
TARGET_DEVICES += tl-wr710n-v1

得到tl-wr740n-v5的机型（target/linux/ar71xx/image/tiny-tp-link.mk）

#define Device/tl-wr740n-v5
#  $(Device/tl-wr740n-v4)
#  DEVICE_TITLE := TP-LINK TL-WR740N/ND v5
#  TPLINK_HWID := 0x07400005
#endef
#TARGET_DEVICES += tl-wr740n-v5

define Device/tl-wr740n-v5
  $(Device/tplink-16mlzma)
  DEVICE_TITLE := TP-LINK TL-WR740N v5 (16MB)
  BOARDNAME := TL-WR710N
  DEVICE_PROFILE := TLWR710
  TPLINK_HWID := 0x07100001
  CONSOLE := ttyATH0,115200
endef
TARGET_DEVICES += tl-wr740n-v5

二、target/linux/ar71xx/base-files/etc/board.d/02_network
将以下：
	tl-wr710n|\
	tl-wr720n-v3|\
	tl-wr810n|\
	tl-wr810n-v2|\
	wpe72|\
	wrtnode2q)
		ucidef_set_interfaces_lan_wan "eth1" "eth0"
		;;
	tl-wr710n)
		ucidef_set_interfaces_lan_wan "eth1.1" "eth0"
		ucidef_add_switch "switch0" \
			"0@eth1" "1:lan:1" "2:lan:2" "3:lan:3" "4:lan:4"
		;;
		
改为（将原有tl-wr710n删除，在末尾添加710n的网口信息）：

	tl-wr720n-v3|\
	tl-wr810n|\
	tl-wr810n-v2|\
	wpe72|\
	wrtnode2q)
		ucidef_set_interfaces_lan_wan "eth1" "eth0"
		;;
	tl-wr710n)
		ucidef_set_interfaces_lan_wan "eth1.1" "eth0"
		ucidef_add_switch "switch0" \
			"0@eth1" "1:lan:1" "2:lan:2" "3:lan:3" "4:lan:4"
		;;
  
  
【2022.1.28】
  1、解决git.openwrt.org无法访问问题
  #src-git routing https://git.openwrt.org/feed/routing.git;openwrt-19.07
  src-git routing https://github.com/openwrt/routing.git;openwrt-19.07
  
  2.新增机型 TL-WDR5800-V1（ar71xx-generic-tl-wdr5800-v1 16MB）:
  CPU：qca9561，RAM：128MB，ROM：16MB，2.4GHz：9561，5GHz：qca9887，面向端口布局：WLLLL
```



欢迎来到Lean的Openwrt源码仓库！
=
Welcome to Lean's  git source of OpenWrt and packages
=
中文：如何编译自己需要的 OpenWrt 固件
-
注意：
-
1. **不**要用 **root** 用户 git 和编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1, 密码 password

编译命令如下:
-
1. 首先装好 Ubuntu 64bit，推荐  Ubuntu  18 LTS x64

2. 命令行输入 `sudo apt-get update` ，然后输入
`
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget
`

3. 使用 `git clone https://github.com/coolsnowwolf/openwrt` 命令下载好源代码，然后 `cd openwrt` 进入目录

4. ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. `make -j8 download V=s` 下载dl库（国内请尽量全局科学上网）


6. 输入 `make -j1 V=s` （-j1 后面是线程数。第一次编译推荐用单线程）即可开始编译你要的固件了。

本套代码保证肯定可以编译成功。里面包括了 R20 所有源代码，包括 IPK 的。

你可以自由使用，但源码编译二次发布请注明我的 GitHub 仓库链接。谢谢合作！
=

二次编译：
```bash
cd openwrt
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j8 download
make -j$(($(nproc) + 1)) V=s
```

如果需要重新配置：
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j$(($(nproc) + 1)) V=s
```

编译完成后输出路径：/openwrt/bin/targets

特别提示：
------
1.源代码中绝不含任何后门和可以监控或者劫持你的 HTTPS 的闭源软件，SSL 安全是互联网最后的壁垒。安全干净才是固件应该做到的；

2.如有技术问题需要讨论，欢迎加入 QQ 讨论群：OP共享技术交流群 ,号码 297253733 ，加群链接: 点击链接加入群聊【OP共享技术交流群】：[点击加入](https://jq.qq.com/?_wv=1027&k=5yCRuXL "OP共享技术交流群")

3.想学习OpenWrt开发，但是摸不着门道？自学没毅力？基础太差？怕太难学不会？跟着佐大学OpenWrt开发入门培训班助你能学有所成
报名地址：[点击报名](http://forgotfun.org/2018/04/openwrt-training-2018.html "报名")

## 软路由介绍
友情推荐不恰饭：如果你在寻找一个低功耗小体积性能不错的 x86/x64 路由器，我个人建议可以考虑 
小马v1 的铝合金版本 (N3710 4千兆)：[页面介绍](https://item.taobao.com/item.htm?spm=a230r.1.14.20.144c763fRkK0VZ&id=561126544764 " 小马v1 的铝合金版本")

![xm1](doc/xm5.jpg)
![xm2](doc/xm6.jpg)

## Donate

如果你觉得此项目对你有帮助，可以捐助我们，以鼓励项目能持续发展，更加完善

### Alipay 支付宝

![alipay](doc/alipay_donate.jpg)

### Wechat 微信

![wechat](doc/wechat_donate.jpg)

------

English Version: How to make your Openwrt firmware.
-
Note:
--
1. DO **NOT** USE **ROOT** USER TO CONFIGURE!!!

2. Login IP is 192.168.1.1 and login password is "password".

Let's start!
---
First, you need a computer with a linux system. It's better to use Ubuntu 18 LTS 64-bit.

Next you need gcc, binutils, bzip2, flex, python3.5+, perl, make, find, grep, diff, unzip, gawk, getopt, subversion, libz-dev and libc headers installed.

To install these program, please login root users and type
`
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib
`
in terminal

Third, logout of root users. And type this `git clone https://github.com/coolsnowwolf/lede` in terminal to clone this source.

After these please type `cd lede` to cd into the source.

Please Run `./scripts/feeds update -a` to get all the latest package definitions
defined in `feeds.conf` / `feeds.conf.default` respectively
and `./scripts/feeds install -a` to install symlinks of all of them into
`package/feeds/` .

Please use `make menuconfig` to choose your preferred
configuration for the toolchain and firmware.

Use `make menuconfig` to configure your image.

Simply running `make` will build your firmware.
It will download all sources, build the cross-compile toolchain,
the kernel and all choosen applications.

To build your own firmware you need to have access to a Linux, BSD or MacOSX system
(case-sensitive filesystem required). Cygwin will not be supported because of
the lack of case sensitiveness in the file system.

## Note: Addition Lean's private package source code in `./package/lean` directory. Use it under GPL v3.

## GPLv3 is compatible with more licenses than GPLv2: it allows you to make combinations with code that has specific kinds of additional requirements that are not in GPLv3 itself. Section 7 has more information about this, including the list of additional requirements that are permitted.
