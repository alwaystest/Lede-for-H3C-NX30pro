# OpenWRT(LEDE) For H3C Magic NX30Pro 
![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Zachery-Liu/Lede-for-H3C-NX30pro/merge-upstream.yml?style=for-the-badge&label=Merge)
![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Zachery-Liu/Lede-for-H3C-NX30pro/openwrt-ci.yml?label=Build&style=for-the-badge)
![GitHub all releases](https://img.shields.io/github/downloads/Zachery-Liu/Lede-for-H3C-NX30pro/total?style=for-the-badge)




这是一个为 H3C Magic NX30Pro 编译固件的库。  
于每天 19:30 自动与上游 [LEDE](https://github.com/coolsnowwolf/lede) 库进行对比，后通过 Github CI 自动拉取新内容并编译发布。

## 下载
见 [CI](https://github.com/Zachery-Liu/Lede-for-H3C-NX30pro/actions/workflows/openwrt-ci.yml) 或 [Release](https://github.com/Zachery-Liu/Lede-for-H3C-NX30pro/releases) 页面。

## 配置信息
设备：H3C Magic NX30 Pro  

固件内编译插件列表：
| 包名 | 说明 |
|:-----:|:-----:|
|luci-app-adbyby-plus|广告屏蔽大师 Plus +|
|luci-app-argon-config|Argon 主题设置|
|luci-app-design-config|Design 主题设置|
|luci-app-easymesh|简单 Mesh|
|luci-app-frpc|Frp 客户端|
|luci-app-ntpc|NTP 时间同步服务器|
|luci-app-openclash|OpenClash 插件|
|luci-app-pushbot|全能推送|
|luci-app-ramfree|内存释放|
|luci-app-statistics|统计工具|
|luci-app-unblockneteasemusic|解锁网易云 (已移除)|
|luci-app-wifischedule|Wifi 计划任务|
|luci-app-wireguard|Wireguard|
|luci-app-zerotier|Zerotier 穿透工具|

此外还有 KMS 服务器、Turbo ACC、上网时间控制、动态 DNS、WiFi 唤醒、UPNP 等默认编译进固件的插件。  

~~L 大原版自带的解锁网易云效果不太好，就更换成了[另外一个](https://github.com/UnblockNeteaseMusic/luci-app-unblockneteasemusic)。~~
这个编译时间太长了 直接删掉好了 不是刚需

## 其他用法
本库可以用于编译其他路由器的固件。  

1.按照 [编译命令](https://github.com/coolsnowwolf/lede#%E7%BC%96%E8%AF%91%E5%91%BD%E4%BB%A4)的前三步完成固件相关自定义配置；


2.使用命令
```bash
make defconfig
./scripts/diffconfig.sh > diff.config
```
  

在文件夹中找到`diff.config`文件并复制全部内容；

3.Fork 本仓库；

4.将`.github/workflows/openwrt-ci.yml`文件中`cat >> .config <<EOF`与`EOF`之间内容修改为第 2 步中`diff.config`文件内容，**注意缩进**。

```yml
      - name: Generate configuration file
        run: |
          rm -f ./.config*
          touch ./.config

          #
          # 在 cat >> .config <<EOF 到 EOF 之间粘贴你的编译配置，需注意缩进关系
          # 例如:

          cat >> .config <<EOF
          
          这里修改为 diff.config 文件内容

          EOF

          #
          # ===============================================================
          # 

          sed -i 's/^[ \t]*//g' ./.config
          make defconfig


```

5.Commit 修改，Action 将自动开始编译，之后可在 Release 中找到相应固件。

## 致谢
感谢 Lean 的  [LEDE](https://github.com/coolsnowwolf/lede)。  

感谢 KFERMercer 的 [OpenWrt-CI](https://github.com/KFERMercer/OpenWrt-CI)。  
