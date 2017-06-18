# aliyun-ddns-client-merlin
这基本上是一个简单的小教程，教你如何在安装了梅林固件的路由器上设置阿里云DDNS。

# 0. 安装梅林固件并配置entware
在这里下载原版梅林固件，并安装到你的路由器：
```
https://asuswrt.lostrealm.ca/download
```

在路由器上插入一个U盘，然后以ssh登陆路由器，键入：
```
entware-setup.sh
```

根据提示完成安装。

```
admin@RT-AC56U:/tmp/home/root/# cd /opt
admin@RT-AC56U:/tmp/mnt/sda1/entware# 
```

# 1. 安装pip，安装requests
```
# opkg install python-pip
# pip install requests
```

# 2. 安装rfancn的客户端

https://github.com/rfancn/aliyun-ddns-client
```
admin@RT-AC56U:/tmp/mnt/sda1/entware# ls
aliyun-ddns-client  etc                 sbin                tmp                 var
bin                 lib                 share               usr
```
重命名 "ddns.conf.example" 为 "ddns.conf"，并按要求配置。
你可能需要在阿里云控制台取得你的 access_id 和 access_key 。

# 3. 设置custom ddns
在路由器上将DDNS设置为custom，并在系统管理中打开 “Enable JFFS custom scripts and configs”。
在jffs分区中新增ddns-start，内容如下：
```
#!/bin/sh
cd /opt/aliyun-ddns-client/ && /opt/bin/python /opt/aliyun-ddns-client/ddns.py
```

# 4. 修改ddns.py，以在web ui中显示ddns状态
引入os模块
```
import os
```

在合适的地方添加以下代码：
```
os.system('/sbin/ddns_custom_updated 0')
os.system('/sbin/ddns_custom_updated 1')
```

# 5. 设置完成
至此已经设置完成，如果解析记录更新成功，web ui中就不会再显示黄色的警告标志。如果无法成功更新，可以手动运行客户端查看错误信息。
