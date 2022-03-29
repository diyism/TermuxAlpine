# TermuxAlpine

#### _powered by_

![powered by Alpine](../master/docs/images/alpinelinux-logo.svg)

This Termux bash setup shell script will attempt to set Alpine Linux up in your Termux environment.

## _Steps For Installation_
1. First go to home directory
 Download and install from https://github.com/termux/termux-app/releases/
 the apk in github only takes 30MB, the apk on f-droid is 100MB, the apk on google play is old/deprecated(lack of left and right arrow keys)
2. Get the script
```
`cd $HOME`
curl -LO https://raw.githubusercontent.com/diyism/TermuxAlpine/master/TermuxAlpine.sh
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

## access /sdcard:
```
cd ~
termux-setup-storage
ls storage
```

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
