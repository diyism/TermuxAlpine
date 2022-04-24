# TermuxXserver

This Termux bash setup shell script will attempt to set Alpine Linux up in your Termux environment.

## _Steps For Installation_
1. Download and install termux from https://github.com/termux/termux-app/releases/
 the apk in github only takes 30MB, the apk on f-droid is 100MB, the apk on google play is old/deprecated(lack of left and right arrow keys)
2. Get the script
```
`cd $HOME`
curl -LO https://raw.githubusercontent.com/diyism/TermuxXserver/master/TermuxAlpine.sh
this fork added "\${PREFIX}/share/TermuxAlpine/tmp:/dev/shm" to support booting chromium
```
3. Execute the script
```
pkg upgrade               # or else when run TermuxAlpine.sh will report libcrypto.so error
bash TermuxAlpine.sh
```
4. Start Alpine
```
$ startalpine
$ cat alpine.src
export XDG_RUNTIME_DIR=/data/data/com.termux/files/home
cd $XDG_RUNTIME_DIR
$ source alpine.src

#apk update
#apk add xfce4 x11vnc xorg-server-xvfb git chromium
#pkg install x11-repo fluxbox x11vnc xorg-server-xvfb git python3
#novnc need python3, fluxbox is 20MB(while both xfce4 and lxqt is over 100MB)

while true; do nohup /usr/bin/x11vnc -noxfixes -usepw -repeat -loop -create -noshm -gone 'killall Xvfb' -env X11VNC_CREATE_STARTING_DISPLAY_NUMBER=0 -env X11VNC_CREATE_GEOM=2560x1688x16 >/dev/null 2>&1; done &
#"-create" means creating an xvfb display
#and we can use "nwrkbiz/android-xserver" to replace "x11vnc and xorg-server-xvfb", the xServer apk create a display at ":0"

#open androidVNC(vnc viewer) app to connecct localhost:5900 or use novnc:
#git clone --depth 1 https://github.com/novnc/noVNC.git
while true; do nohup /data/data/com.termux/files/home/noVNC/utils/novnc_proxy --vnc localhost:5900 --listen 6081 >/dev/null 2>&1; sleep 1; done &
#xfce4-seesion need get rid of "nohup" and "&"
while true; do /data/data/com.termux/files/usr/bin/xfce4-session --display=:0  >/dev/null 2>&1; sleep 1; done
# visit http://127.0.0.1:6081/vnc.html on android browser
# it's weird that while clicking the "Connect" button in vnc.html the "xfdesktop" process will occupy 3GB more memory(maybe killed by android system), and 5 seconds later Termux will release all memory.

$ cat x11vnc.sh
#!/bin/bash

#====="-noshm" is a must for alpine in termux:
while true; do nohup /usr/bin/x11vnc -noxfixes -usepw -repeat -loop -create -noshm -gone 'killall Xvfb' -env X11VNC_CREATE_STARTING_DISPLAY_NUMBER=0 -env X11VNC_CREATE_GEOM=2560x1688x16 >/dev/null 2>&1; sleep 1; done &
while true; do nohup /data/data/com.termux/files/home/noVNC/utils/novnc_proxy --vnc localhost:5900 --listen 6081 >/dev/null 2>&1; sleep 1; done &

#=====for termux, there is xorg-server-xvfb without /usr/bin/xinit:
#xfce4-seesion need get rid of "nohup" and "&"
#while true; do /usr/bin/xfce4-session --display=:0  >/dev/null 2>&1; sleep 1; done
#=====for alpine in termux, there is only Xvfb with a must /usr/bin/xinit:
#echo 'exec xfce4-session' > /data/data/com.termux/files/home/.xinitrc

#start chromium:
env DISPLAY=:0 /usr/lib/chromium/chromium-lancher.sh --no-sandbox

```
5. For exit just execute
`exit`

## gowebview+xserver:
```
#install "nwrkbiz/android-xserver" apk and start it
pkg install x11-repo golang webkit2gtk xorgproto glib-networking
#startalpine
#apk add gcc-go webkit2gtk-dev
go get github.com/webview/webview
export DISPLAY=:0
go run test.go
#now I can see the web page in "nwrkbiz/android-xserver" app
```

## access /sdcard:
```
cd ~
termux-setup-storage
ls storage
ls /sdcard

termux-backup /sdcard/termux-backup.pkgs.tar.xz
#termux-restore /sdcard/termux-backup.pkgs.tar.xz
```

## cloudflared tunnel:
```
#we need compile cloudflared by ourselve
$ pkg install golang git libcurl debianutils make
#libcurl need be upgraded, or else git show error "library "libssl.so.1.1" not found"
$ git clone --depth=1 https://github.com/cloudflare/cloudflared.git
$ cd cloudflared
$ sed -i 's/linux/android/g' Makefile
$ make cloudflared
$ install cloudflared /data/data/com.termux/files/usr/bin/

# don't use cloudflared-linux-arm64, will happen error to use /etc/resolv.conf which doesn't exist in android
# wget https://github.com/cloudflare/cloudflared/releases/download/2022.4.1/cloudflared-linux-arm64

$ wget https://github.com/diyism/TermuxXserver/releases/download/test/termux-cloudflared
$ install termux-cloudflared /data/data/com.termux/files/usr/bin/cloudflared
$ cloudflared tunnel login             #it will auto open android browser from termux
$ cloudflared tunnel create www1
$ cloudflared tunnel route dns www1 www1.gvgle.com
$ nano ~/.cloudflared/www1.yml
url: http://localhost:3000
tunnel: <tunnel id>
credentials-file: /data/data/com.termux/files/home/.cloudflared/<tunnel id>.json
$ ./cloudflared-linux-arm64 tunnel --config ~/.cloudflared/www1.yml run
$ wget https://github.com/xyproto/algernon/releases/download/1.12.14/algernon-1.12.14-linux_arm64.tar.xz
$ tar xf algernon-1.12.14-linux_arm64.tar.xz
$ ./algernon-1.12.14-linux_arm64/algernon
```
now you can visit https://www1.gvgle.com  from you browser, it serves from algernon web server in termux os

## _Steps For First Time Use (Recommended)_
1. Update Alpine
`apk update`
2. Now you can install any package by
`apk add package_name`

## Size Comparision
Size  | Alpine  | Arch | Ubuntu
--- | --- | --- | ---
before installation | Around 1 MB 😱  | Around 400 MB | Around 35 MB
after installation | Around 80 MB | Around 2000 MB | Around 1200 MB

#### here is full usage details of apk https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management


Comments are welcome at https://github.com/Hax4us/TermuxAlpine/issues ✍

Pull requests are welcome at https://github.com/Hax4us/TermuxAlpine/pulls ✍
