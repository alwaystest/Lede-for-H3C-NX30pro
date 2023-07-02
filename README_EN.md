# OpenWRT(LEDE) For H3C Magic NX30Pro 
![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Zachery-Liu/Lede-for-H3C-NX30pro/merge-upstream.yml?style=for-the-badge&label=Merge)
![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Zachery-Liu/Lede-for-H3C-NX30pro/openwrt-ci.yml?label=Build&style=for-the-badge)
![GitHub all releases](https://img.shields.io/github/downloads/Zachery-Liu/Lede-for-H3C-NX30pro/total?style=for-the-badge)




This is a library to compile firmware for H3C Magic NX30Pro.  
It is automatically compared with the upstream [LEDE](https://github.com/coolsnowwolf/lede) library at 19:30 every day, and then the new content is automatically pulled and compiled for release via Github CI.

## Download
See [CI](https://github.com/Zachery-Liu/Lede-for-H3C-NX30pro/actions/workflows/openwrt-ci.yml) or [Release](https://github.com/Zachery-Liu/Lede-for-H3C-NX30pro/releases) page.

## Configuration Information
Device: H3C Magic NX30 Pro  

List of compiled plug-ins within the firmware:
| Package Name | Description |
|:-----:|:-----:|
|luci-app-adbyby-plus|Adblock Master Plus +|
|luci-app-argon-config|Argon theme settings|
|luci-app-design-config|Design theme settings|
|luci-app-easymesh|Simple Mesh|
|luci-app-frpc|Frp client|
|luci-app-ntpc|NTP time synchronization server|
|luci-app-openclash|OpenClash plug-in|
|luci-app-pushbot|All-purpose push|
|luci-app-ramfree|memory release|
|luci-app-statistics|statistics tools|
|luci-app-unblockmusic|unblock Netflix (removed from subsequent firmware)|
|luci-app-wifischedule|Wifi scheduled tasks|
|luci-app-wireguard|Wireguard|
|luci-app-zerotier|Zerotier penetration tool|

There are also plugins for KMS Server, Turbo ACC, Internet Time Control, Dynamic DNS, WiFi Wakeup, UPNP, etc. that are compiled into the firmware by default.  

Unlock Netflix doesn't work very well, so it's not built in. You can use `opkg` to install it again if you need it.

## Other usage
This library can be used to compile firmware for other routers.  

1. follow the first three steps of the [compile command](https://github.com/coolsnowwolf/lede#%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4) to complete the firmware-related custom configuration;


2. Use the command

```bash
make defconfig
. /scripts/diffconfig.sh > diff.config
```
  

Locate the ``diff.config`` file in the folder and copy the entire contents;

3. Fork this repository;

4. Modify the contents between `.github/workflows/openwrt-ci.yml` file line 74 `cat >> .config <<EOF` and line 152 `EOF` to the contents of `diff.config` file in step 2, **Note the indentation**.

```yml
      - name: Generate configuration file
        run: |
          rm -f . /.config*
          touch . /.config

          #
          # Paste your compiled configuration between cat >> .config <<EOF to EOF, taking care of the indentation
          # For example.

          cat >> .config <<EOF
          
          Here the contents of the diff.config file are modified

          EOF

          #EOF
          # ===============================================================
          # 

          sed -i 's/^[ \t]*//g' . /.config
          make defconfig
```

5. Commit the changes and Action will automatically start compiling, after which the corresponding firmware can be found in the Release.

## Acknowledgements
Thanks to Lean for [LEDE](https://github.com/coolsnowwolf/lede).
Thanks to KFERMercer for [OpenWrt-CI](https://github.com/KFERMercer/OpenWrt-CI).
Translated with www.DeepL.com/Translator (free version).