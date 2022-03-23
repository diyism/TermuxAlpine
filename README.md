# TermuxAlpine

#### _powered by_

![powered by Alpine](../master/docs/images/alpinelinux-logo.svg)

This Termux bash setup shell script will attempt to set Alpine Linux up in your Termux environment.

## _Steps For Installation_
1. First go to home directory
`cd $HOME`
2. Get the script
`curl -LO https://raw.githubusercontent.com/Hax4us/TermuxAlpine/master/TermuxAlpine.sh`
3. Execute the script
```
pkg upgrade               # or else when run TermuxAlpine.sh will report libcrypto.so error
bash TermuxAlpine.sh
```
4. Start Alpine
```
#pkg install x11-repo termux-x11 dpkg wget
#wget https://github.com/termux/termux-x11/releases/download/1.02.06/termux-x11.deb
#dpkg -i termux-x11.deb     #and install termux-x11.apk into android and start it and approve permissions
#nano .termux/termux.properties       #uncomment "allow-external-apps true"
#export XDG_RUNTIME_DIR=/data/data/com.termux/files/home
#termux-x11 :0 >/dev/null &                     #start Xwayland (to replace xorg-server, need no installing xorg-server)

startalpine
export XDG_RUNTIME_DIR=/data/data/com.termux/files/home
apk update
apk add xfce4 x11vnc xorg-server-xvfb git
while true; do nohup /usr/bin/x11vnc -noxfixes -usepw -repeat -loop -create -gone 'killall Xvfb' -env X11VNC_CREATE_STARTING_DISPLAY_NUMBER=0 -env X11VNC_CREATE_GEOM=2560x1688x16 >/dev/null 2>&1; done &
#"-create" means creating an xvfb display
#open androidVNC(vnc viewer) app to connecct localhost:5900 or use novnc:
git clone --depth 1 https://github.com/novnc/noVNC.git
while true; do nohup /data/data/com.termux/files/home/noVNC/utils/novnc_proxy --vnc localhost:5900 --listen 6081 >/dev/null 2>&1; sleep 1; done &
#xfce4-seesion need get rid of "nohup" and "&"
while true; do /data/data/com.termux/files/usr/bin/xfce4-session --display=:0  >/dev/null 2>&1; sleep 1; done
# visit http://127.0.0.1:6081/vnc.html on android browser
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
