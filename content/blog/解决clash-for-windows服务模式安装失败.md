---
title: "解决clash for Windows服务模式安装失败"
date: 2023-09-16T16:08:31+08:00
draft: false
tags: ["archlinux", "clash-for-windows"]
---

## 设置服务模式

之前安装了[xerolinux](https://xerolinux.xyz/)，因为安装[clash-for-windows](https://docs.cfw.lbyczf.com/)安装服务模式启动失败，一直很烦，后面直接放弃了。

既然是为了想一个安稳的环境，何不使用[ubuntu](https://cn.ubuntu.com/)，可是我哪里是什么良人，对于见过[xeroLinux](https://xerolinux.xyz/)美貌的人，我真的忍受不了这戒色戒欲的古板桌面；当然，折腾是不可能折腾的，否则都自己原装[arch](https://www.archlinuxcn.org/)美化了。

重新安装好系统之后，耐心的等待十分钟后，`clash-for-windows`再次回到了我的电脑，看到熟悉的页面，果然还是熟悉的服务模式安装失败。

继续耐心的寻找解决方案

1. 找到[线索1](https://docs.cfw.lbyczf.com/contents/questions.html#service-mode-%E6%97%A0%E6%B3%95%E5%AE%89%E8%A3%85)
2. 索引之后找到[线索2](https://github.com/Fndroid/clash_for_windows_pkg/issues/3464)，不过看这路径，完全匹配不上，心态要炸了
3. 耐心看到最后，终于看到了解决方案，果然是[神教](https://www.zhihu.com/question/49056249)万岁

## 具体流程

1. 编辑服务
```shell
# /usr/lib/systemd/system/clash-core-service.service
[Unit]
Description=Clash core service created by Clash for Windows
After=network-online.target nftables.service iptables.service

[Service]
Type=simple
ExecStart=/opt/clash-for-windows-bin/resources/static/files/linux/x64/service/clash-core-service
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```
2. 重启服务
```shell
sudo systemctl enable clash-core-service
sudo systemctl start clash-core-service
```

