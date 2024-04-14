# oneCloud
oneCloud built with armbian

#### 安装armbian
[玩客云安装casaos、Docker、qbittorrent](https://zhuanlan.zhihu.com/p/658340519?utm_medium=social&utm_oi=697147908176224256&utm_psn=1748803540695527424&utm_source=wechat_session)

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
- DrawIO:
- ```
  docker pull doctorpeso/drawio-arm
  docker run -dit --name=draw-io -p 8080:8080 doctorpeso/drawio-arm
  ```

- EmulatorJS
  ```
  docker pull linuxserver/emulatorjs
  docker run -d --name emulatorjs -e PUID=1000 -e PGID=1000 -e TZ=Asia/Shanghai -p 80:80 -p 3000:3000 -v /var/www/emulatorjs/config:/config -v /var/www/emulatorjs/data:/data linuxserver/emulatorjs
  ```
