---
title: Mac：为终端设置Shadowsocks代理
date: 2019-02-18 20:57:39
tags:  
- 网络代理
- Shadowsocks
categories: 
- 技术
copyright: true
---
Shadowsocks使用socks5协议，而终端很多工具目前只支持http和https等协议，所以我们为终端设置Shadowsocks的思路就是将socks5协议转换成http协议，然后为终端设置即可。
使用 [HomeBrew](https://brew.sh/) 安装 [polipo](http://www.pps.univ-paris-diderot.fr/~jch/software/polipo/),
``` Terminal
$ brew install polipo
```
## 修改`polipo`配置
1.设置每次打开电脑自动启动` polipo `
``` 
$ ln -sfv /usr/local/opt/polipo/*.plist ~/Library/LaunchAgents
```
2.修改文件`/usr/local/opt/polipo/homebrew.mxcl.polipo.plist`设置`parentProxy`
  先查看Shadowsocks的偏好设置，然后点击HTTP
![Pasted Graphic 1.png](https://upload-images.jianshu.io/upload_images/138050-f0a83e2b25e0866e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
添加`<string>socksParentProxy=localhost:1087</string>`  
```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Label</key>
    <string>homebrew.mxcl.polipo</string>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/opt/polipo/bin/polipo</string>
        <!-- 添加如下一行 -->
        <string>socksParentProxy=localhost:1087</string>
    </array>
    <!-- Set `ulimit -n 20480`. The default OS X limit is 256, that's
         not enough for Polipo (displays 'too many files open' errors).
         It seems like you have no reason to lower this limit
         (and unlikely will want to raise it). -->
    <key>SoftResourceLimits</key>
    <dict>
      <key>NumberOfFiles</key>
      <integer>20480</integer>
    </dict>
  </dict>
</plist>
```
## 配置`.bash_profile`文件

**为了让如下配置在全局有效，需将如下配置添加到.bash_profile文件。**
打开`~/.bash_profile`文件(如果不存在此文件，使用`touch .bash_profile`命令创建)
添加如下两行配置
```
# 开启代理
alias hp="export http_proxy='http://localhost:1087';export https_proxy='http://localhost:1087'"
# 关闭代理
alias hpOff="unset http_proxy; unset https_proxy"

# 以上hp,hpOff命名可根据个人习惯命名  hp代表http proxy简称...
```
然后使用`$source ~/.bash_profile`使上面两行配置起效。
## 验证
如想让终端走`Showdowsocks`代理，执行`$ hp`指令
如不想让终端走`Showdowsocks`代理，执行`$ hpOff`指令
验证以上指令是否有效,可在终端执行`$ curl ip.gs  `查看ip信息
```
$ curl ip.gs   // 查看ip信息
Current IP / 当前 IP: 111.206.170.9
ISP / 运营商:  ChinaUnicom
City / 城市: Beijing Beijing
Country / 国家: China
IP.GS is now IP.SB, please visit https://ip.sb/ for more information. / IP.GS 已更改为 IP.SB ，请访问 https://ip.sb/ 获取更详细 IP 信息！
Please join Telegram group https://t.me/sbfans if you have any issues. / 如有问题，请加入 Telegram 群 https://t.me/sbfans 
  /\_/\
=( °w° )=
  )   (  //
 (__ __)//

$ hp
$ curl ip.gs  
Current IP / 当前 IP: 104.245.13.64
ISP / 运营商:  xtom.com
City / 城市: Los Angeles California
Country / 国家: United States
IP.GS is now IP.SB, please visit https://ip.sb/ for more information. / IP.GS 已更改为 IP.SB ，请访问 https://ip.sb/ 获取更详细 IP 信息！
Please join Telegram group https://t.me/sbfans if you have any issues. / 如有问题，请加入 Telegram 群 https://t.me/sbfans 

  /\_/\
=( °w° )=
  )   (  //
 (__ __)//
```
## 设置git代理
操作git是在后面添加配置 `--config http.proxy=localhost:1087`，例如：
```
git clone https://github.com/usename/projectName.git --config http.proxy=localhost:1087
```
可把以上指令添加到`.bash_profile`文件,这样就避免每次输入这么长的配置了。
```
gp=" --config http.proxy=localhost:8123"
```
简化后：
```
git clone https://github.com/usename/projectName.git $gp
```

