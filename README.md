# oneCloud
oneCloud built with armbian

#### 安装armbian
[玩客云安装casaos、Docker、qbittorrent](https://zhuanlan.zhihu.com/p/658340519?utm_medium=social&utm_oi=697147908176224256&utm_psn=1748803540695527424&utm_source=wechat_session)

[玩客云刷Armbian，安装docker+CasaOS+OpenWrt](https://zhuanlan.zhihu.com/p/603845854)

[玩客云刷 Armbian 系统，装 Docker/Portainer/Home Assistant 教程](https://mp.weixin.qq.com/s?__biz=MzIxOTE5MDY5Mw==&mid=2650966188&idx=1&sn=ef0558993b5d48a12867f54a65d8e35a&chksm=8c292c66bb5ea5709889351720e85360d2bcbc3176bf002ef8aa14e343e3b49dbf58dc255d0e&scene=21#wechat_redirect)

[【2024年2月1日 】玩客云刷Debian小白保姆级教程AllinOne：Debian23.11+CasaOS+Docker+LED灯控制+QBitTorrent+Cpolar内网穿透+青龙+Home Assistant智能家居+Wol远程唤醒+N](https://www.right.com.cn/forum/thread-8344722-1-1.html)
#### 修改面板灯光
把下面代码写进/etc/rc.local
```
echo 1 > /sys/class/leds/onecloud:blue:alive/brightness
echo 0 > /sys/class/leds/onecloud:green:alive/brightness
echo 0 > /sys/class/leds/onecloud:red:alive/brightness
```

#### 格式化、挂载、卸载硬盘
查询可用硬盘(sdb1为未挂载硬盘): `lsblk`
```
root@onecloud:~# lsblk
NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda            8:0    0 465.8G  0 disk
└─sda1         8:1    0 465.8G  0 part /media/devmon/sda1-ata-TOSHIBA_MQ01ABD0
sdb            8:16   0 931.5G  0 disk
└─sdb1         8:17   0 931.5G  0 part
mmcblk1      179:0    0   7.3G  0 disk
├─mmcblk1p1  179:1    0   256M  0 part /boot
└─mmcblk1p2  179:2    0     7G  0 part /var/log.hdd
...
root@onecloud:~#
```

查询硬盘UUID: `blkid`
```
root@onecloud:~# blkid
/dev/sdb1: UUID="79d7fe84-b0fc-4d07-94bf-744fd75c1b2f" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="856e932e-01"
...
```

格式化硬盘为ext4格式: `mkfs.ext4 /dev/sda1`

挂载硬盘: `mount UUID="1f98824c-0f5d-456a-9df7-d24e81d9a8a6"`或`mount /dev/sdb1`

卸载硬盘: `umount /dev/sdb1`

#### 内网穿透
推荐 [tailscale](https://tailscale.com/), 简单方便

#### Docker应用推荐
- [三方App市场 big-bear-casaos](https://github.com/bigbeartechworld/big-bear-casaos/tree/master)
- DrawIO:
  ```
  docker pull doctorpeso/drawio-arm
  docker run -dit --name=draw-io -p 8080:8080 doctorpeso/drawio-arm
  ```
- EmulatorJS (目前不支持arm)
  ```
  # https://www.bilibili.com/read/cv21205700/
  # https://blog.csdn.net/weixin_56714594/article/details/133909332
  docker pull linuxserver/emulatorjs
  docker run -d --name emulatorjs -e PUID=1000 -e PGID=1000 -e TZ=Asia/Shanghai -p 80:80 -p 3000:3000 -v /var/www/emulatorjs/config:/config -v /var/www/emulatorjs/data:/data linuxserver/emulatorjs
  ```
- Memos (armv7) [在玩客云上用Docker部署memos](https://ruohai.wang/202311/memos-install-on-onecloud/)
  ```
  docker run -d \
    --init \
    --name memos \
    --publish 5230:5230 \
    --volume /media/devmon/sda1-ata-TOSHIBA_MQ01ABD0/docker-volumes/memos/:/var/opt/memos \
    neosmemo/memos:0.15.0
  ```
- QBitTorrent
- Portainer
- Uptime Kuma
  - 消息推送
    - [ServerChan (Server酱)](https://sct.ftqq.com/) 实现微信推送通知
    - [PushDeer](https://www.pushdeer.com/) 可自部署的轻App通知
- HomeAssistant
  ```bash
  docker run -d \
  --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -e TZ=Asia/Shanghai \
  -v /media/devmon/sda1-ata-TOSHIBA_MQ01ABD0/docker-volumes/HomeAssistant/config:/config \
  --network=host \
  homeassistant/home-assistant:stable
  ```
- [Stirling-PDF](https://github.com/Stirling-Tools/Stirling-PDF)
- [searXNG](https://github.com/searxng/searxng-docker)
- [TaleBook](https://github.com/talebook/talebook)
- [vaultwarden](https://github.com/dani-garcia/vaultwarden)
- AdGuard Home
- [qingfeng2336/it-tools](https://hub.docker.com/r/qingfeng2336/it-tools): 汉化版, 且重新编译支持armv7
- [Mind](https://github.com/Casvt/MIND)
- [青龙面板](https://github.com/whyour/qinglong)
- [excalidraw](https://github.com/excalidraw/excalidraw)
- [excalidraw-docker](https://hub.docker.com/r/excalidraw/excalidraw/tags)
- [chrome-novnc(browser in browser)](https://github.com/vital987/chrome-novnc?tab=readme-ov-file)
