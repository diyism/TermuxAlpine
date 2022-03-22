# TermuxAlpine

#### _powered by_

![powered by Alpine](../master/docs/images/alpinelinux-logo.svg)

![Optional Text](../master/docs/images/ss.png)


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
pkg install x11-repo termux-x11 dpkg wget
wget https://github.com/termux/termux-x11/releases/download/1.02.06/termux-x11.deb
dpkg -i termux-x11.deb
#install termux-x11.apk into android and start it and approve permissions
nano .termux/termux.properties       #uncomment "allow-external-apps true"
export XDG_RUNTIME_DIR=${TMPDIR}
termux-x11 :0 &

startalpine
apk update
apk add xfce4 xorg-server x11vnc git
startxfce4         #env DISPLAY=:0 xfce4-session
x11vnc -localhost -loop
git clone --depth 1 https://github.com/novnc/noVNC.git
./noVNC/utils/novnc_proxy --vnc localhost:5900 --listen 6081
```
5. For exit just execute
`exit`

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
