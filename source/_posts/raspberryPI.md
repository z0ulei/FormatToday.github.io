---
title: 树莓派自用备忘录
date: 2018-08-27 16:03:13
tags: 树莓派
categories: 备忘录
password: pi
---
自用备忘
<!-- more -->

# 下载工具

1. [系统镜像 (Raspbian Stretch with desktop)](https://downloads.raspberrypi.org/raspbian_latest)
2. [内存卡格式化工具 (SDFormatter)](https://pan.baidu.com/s/1qZPgtEs)  密码：o7gr
3. [写镜像工具 (Win32DiskImager)](https://pan.baidu.com/s/1htYQdvQ)  密码：2n6m

# 写入系统

1. 用 SDFormatter 格式化sd卡
2. 用 Win32DiskImager 写入系统镜像
3. boot分区添加**ssh**文件

# 修改系统

## 修改源

```
sudo nano /etc/apt/sources.list
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi

------------------------------

sudo nano /etc/apt/sources.list.d/raspi.list

deb http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ stretch main ui
deb-src http://mirrors.ustc.edu.cn/archive.raspberrypi.org/debian/ stretch main ui
```

## 更新系统

```
sudo apt update
sudo apt upgrade -y
sudo reboot
```

## 更改设置

1. 语言，时区

   ```
   sudo apt install -y ttf-wqy-zenhei
   sudo apt install -y scim-pinyin
   sudo raspi-config
   	Localisation Options->
        	Change Locale
       	Change Timezone
   sudo reboot
   ```

2. 更改usb电流

   ```
   sudo nano /boot/config.txt
   
   max_usb_current=1
   ```

3. 查看磁盘信息

   ```
   sudo fdisk –l
   sudo blkid
   ```

4. 挂载u盘

   ```
   sudo nano /etc/fstab
   /dev/sda1      /home/pi/usb       ext4   defaults,noatime                    0   0
   sudo reboot
   ```

   ​

# aria2下载配置

1. 下载

   ```
   sudo apt install aria2
   ```

2. 创建 配置文件

   ```
   sudo mkdir /home/pi/.aria2
   sudo mkdir /home/pi/usb/download
   sudo touch /home/pi/.aria2/aria2.session
   sudo nano /home/pi/.aria2/aria2.conf
   ```

   ​

3. 配置

   ```
   # 基本配置
   # 下载目录
   dir=/home/pi/usb/download
   # 下载从这个文件中找到的urls, 需自己建立这个文件
   input-file=/home/pi/.aria2/aria2.session
   # 最大同时下载任务数，默认 5
   #max-concurrent-downloads=5
   # 断点续传，只适用于 HTTP(S)/FTP
   continue=true
   # HTTP/FTP 配置
   # 关闭连接如果下载速度等于或低于这个值，默认 0
   #lowest-speed-limit=0
   # 对于每个下载在同一个服务器上的连接数，默认 1
   max-connection-per-server=10
   # 每个文件最小分片大小，例如文件 20M，设置 size 为 10M, 则用2个连接下载，默认 20M
   #min-split-size=10M
   # 下载一个文件的连接数，默认 5
   #split=10
   # RPC 配置
   # 开启 JSON-RPC/XML-RPC 服务，默认 false
   enable-rpc=true
   # 允许所有来源，web 界面跨域权限需要，默认 false
   rpc-allow-origin-all=true
   # 允许外部访问，默认 false
   rpc-listen-all=true
   # rpc 端口，默认 6800
   rpc-listen-port=6800
   # 设置最大的 JSON-RPC/XML-RPC 请求大小，默认 2M
   #rpc-max-request-size=2M
   # rpc token验证
   #rpc-secret=FormatToday
   # 高级配置
   # This is useful if you have to use broken DNS and
   # want to avoid terribly slow AAAA record lookup.
   # 默认 false
   disable-ipv6=true
   # 指定文件分配方法，预分配能有效降低文件碎片，提高磁盘性能，缺点是预分配时间稍长
   # 如果使用新的文件系统，例如 ext4 (with extents support), btrfs, xfs or NTFS(MinGW build only), falloc 是最好的选择
   # NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
   # 如果设置为 none，那么不预先分配文件空间，默认 prealloc
   file-allocation=trunc
   # 整体下载速度限制，默认 0
   #max-overall-download-limit=0
   # 每个下载下载速度限制，默认 0
   #max-download-limit=0
   # 保存错误或者未完成的下载到这个文件
   # 和基本配置中的 input-file 一起使用，那么重启后仍可继续下载
   save-session=/home/pi/.aria2/aria2.session
   # 每5分钟自动保存错误或未完成的下载，如果为 0, 只有 aria2 正常退出才回保存，默认 0
   save-session-interval=300
   # BT 特殊配置
   # 当下载的是一个种子(以.torrent结尾)时, 自动开始BT任务, 默认:true
   follow-torrent=true
   # BT监听端口, 当端口被屏蔽时使用, 默认:6881-6999
   #listen-port=51413
   # 单个种子最大连接数, 默认:55
   #bt-max-peers=55
   # 打开DHT功能, PT需要禁用, 默认:true
   enable-dht=true
   # 打开IPv6 DHT功能, PT需要禁用
   enable-dht6=true
   # DHT网络监听端口, 默认:6881-6999
   #dht-listen-port=6881-6999
   # 本地节点查找, PT需要禁用, 默认:false
   bt-enable-lpd=true
   # 种子交换, PT需要禁用, 默认:true
   enable-peer-exchange=true
   # 每个种子限速, 对少种的PT很有用, 默认:50K
   #bt-request-peer-speed-limit=50K
   # 客户端伪装, PT需要
   peer-id-prefix=-TR2770-
   user-agent=Transmission/2.77
   # 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
   seed-ratio=1.0
   # 强制保存会话, 即使任务已经完成, 默认:false
   # 较新的版本开启后会在任务完成后依然保留.aria2文件
   force-save=true
   # BT校验相关, 默认:true
   #bt-hash-check-seed=true
   # 继续之前的BT任务时, 无需再次校验, 默认:false
   bt-seed-unverified=true
   # 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
   bt-save-metadata=true
   #bt-tracker=
   ```

4. 运行 aria2, 测试配置是否有错误，如果没有提示任何错误信息，那就按Ctrl+C停止。

   ```
   aria2c --conf-path=/home/pi/.aria2/aria2.conf
   ```

5. 为 aria2 添加自启动服务

   ```
   sudo nano /etc/init.d/aria2c
   ```

   ```
   #! /bin/sh
   # /etc/init.d/aria2c
   ### BEGIN INIT INFO
   # Provides: aria2c
   # Required-Start:    $network $local_fs $remote_fs
   # Required-Stop:     $network $local_fs $remote_fs
   # Default-Start:     2 3 4 5
   # Default-Stop:      0 1 6
   # Short-Description: aria2c RPC init script.
   # Description: Starts and stops aria2 RPC services.
   ### END INIT INFO
   #VAR
   RUN="/usr/bin/aria2c"
   ARIA_PID=$(ps ux | awk '/aria2c --daemon=true --enable-rpc/ && !/awk/ {print $2}')
   # Carry out specific functions when asked to by the system
   case "$1" in
     start)
       echo "Starting script aria2c "
       if [ -z "$ARIA_PID" ]; then
         $RUN --daemon=true --enable-rpc=true -D --conf-path=/home/pi/.aria2/aria2.conf
         echo "Started"
       else
         echo "aria2c already started"
       fi
       ;;
     stop)
       echo "Stopping script aria2c"
       if [ ! -z "$ARIA_PID" ]; then
         kill $ARIA_PID
       fi
       echo "OK"
       ;;
     restart)
       echo "Restarting script aria2c"
       if [ ! -z "$ARIA_PID" ]; then
         kill $ARIA_PID
       fi
       sleep 3   # TODO:Maybe need to be adjust
       $RUN --daemon=true --enable-rpc=true -D --conf-path=/home/pi/.aria2/aria2.conf
       echo "OK"
       ;;
     status)
       if [ ! -z "$ARIA_PID" ]; then
         echo "The aria2c is running with PID = "$ARIA_PID
       else
         echo "No process found for aria2c RPC"
       fi
       ;;
     *)
       echo "Usage: /etc/init.d/aria2c {start|stop|restart|status}"
       exit 1
       ;;
   esac
   exit 0
   ```

   ```
   sudo chmod +x /etc/init.d/aria2c
   sudo update-rc.d aria2c defaults
   sudo service aria2c restart
   ```

6. 安装web 前端yaaw或者webui-aria2来实现web管理

   ```
   sudo apt install nginx
   ```

   ```
   sudo nano /etc/nginx/nginx.conf 
   ```

   ```
   user www-data; #默认以www-data运行工作进程
   worker_processes auto; #单工作进程足够了，就我自己访问
   worker_connections 256; #一般支持100在线连接就达到raspberry pi的极限了
   gzip on;  #gzip开启取消前面的#让默认设置生效即可，可以加快网页访问速度
   gzip_disable "msie6";
   gzip_vary on;
   gzip_proxied any;
   gzip_comp_level 6;
   gzip_buffers 16 8k;
   gzip_http_version 1.1;
   gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
   ```

7. 配置站点属性

   ```
   sudo mkdir /var/www && sudo mkdir /var/www/html
   sudo chown -R www-data:www-data /var/www/html
   sudo chmod -R 0755 /var/www/html
   ```

8. 下载webui-aria2

   ```
   cd /var/www/html
   sudo git clone https://github.com/ziahamza/webui-aria2.git
   sudo mv webui-aria2 webui
   sudo service nginx restart
   
   如果提示链接到aria2 RPC server失败，重启aria2c服务，然后刷新网页即可。
   sudo service aria2c restart
   ```

# 安装MongoDB

```
   sudo apt install mongodb
```

   ​

# 安装MySQL

```
https://dev.mysql.com/downloads/repo/apt/

sudo dpkg -i mysql-apt-***.deb
sudo apt-get update

sudo apt-get install mysql-server
# 连接数据库
use mysql;
# 修改密码为123456，自己定
UPDATE user SET password=PASSWORD('123456') WHERE user='root';
# 改外网访问权限
select user,host from user;
# 刷新
flush privileges;
update user set host='%' where user='root';
# 刷新
flush privileges;
# 授权用户 任意主机以用户root和密码pwd连接到mysql服务器
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'pwd' WITH GRANT OPTION;
# 刷新
flush privileges;
# 退出
exit;
操作数据库服务
sudo /etc/init.d/mysql status
sudo /etc/init.d/mysql start
sudo /etc/init.d/mysql stop
sudo /etc/init.d/mysql restart

```

