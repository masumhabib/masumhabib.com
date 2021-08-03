Sharing Desktop Using VNC Server
################################
:date: 2014-10-27 00:13
:author: masum
:category: Blog, How To, Linux
:slug: 1469
:status: draft

We want to share a desktop a user already logged in using VNC server.

| VNC server
|  ----------
| 
  http://techronicles.net/2012/12/23/vnc-x11vnc-over-ssh-tunnel-on-ubuntu-12-10-and-nexus-7-as-a-client/

| Install: X11VNC
|  sudo apt-get install x11vnc

| Set password:
|  sudo x11vnc -storepasswd /etc/x11vnc/pass

sudo vi /etc/init/x11vnc.conf

| start on login-session-start
|  script
|  x11vnc -xkb -noxrecord -noxfixes -noxdamage -display :0 -rfbauth
  /etc/x11vnc/pass -auth /var/run/lightdm/root/:0 -forever -bg -o
  /var/log/x11vnc.log -rfbport 5937
|  end script

Reboot.

| SSH Tunnel
|  http://linuxaria.com/howto/permanent-ssh-tunnels-with-autossh

| on target
|  # useradd -m -s /bin/false -r autossh
|  # su -s /bin/bash autossh
|  $ ssh-kegen

| on server
|  useradd -m -s /bin/false -r autossh

| groupadd -r ssh\_allowed
|  usermod -a -G ssh\_allowed autossh

| vi /usr/local/scripts/vnc\_proxy
|  su -s /bin/sh autossh -c 'autossh -M 0 -q -f -N -o
  "ServerAliveInterval 60" -o "ServerAliveCountMax 3" -R
  5937:localhost:5937 autossh@proxy.masumhabib.com -p 3571'

| vi /etc/ssh/sshd\_config
|  Port 3571
|  AddressFamily inet
|  AllowGroups ssh\_allowed

| Copy contents of target:/home/autossh/.ssh/id\_rsa.pub
|  and paste it to server:/home/autossh/.ssh/authorizes\_keys
