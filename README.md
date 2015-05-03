## ssh-tunnel
Creates SSH reverse tunnel from IoT device to cloud server for remote access.

Provides remote access to IoT devices behind firewall via cloud server with public Internet IP address.

Each IoT device appears mapped to selected port number on cloud server.

Mapped ports are localhost on server for security so must log in to cloud server to access remote devices.

Known working on IoT platforms:

* Raspberry Pi 2 (Debian 7.8 Wheezy)
* BeagleBone Black (Debian 7.8 Wheezy)

#### Installation

Install autossh:
```
$ sudo apt-get install autossh
```
Copy ssh-tunnel to /etc/init.d/ssh-tunnel.

Define HOST, PORT, and USER in /etc/init.d/ssh-tunnel.

Define equivalent HOST in /etc/hosts.

Eliminate need for interactive password (as root since runs as root at startup):
```
$ sudo ssh-keygen -t rsa
$ sudo ssh-copy-id $USER@$HOST
```
Commands to update sysvinit script links:
```
$ sudo update-rc.d ssh-tunnel defaults
$ sudo update-rc.d ssh-tunnel disable
$ sudo update-rc.d ssh-tunnel remove
```
Commands to start/stop service:
```
$ sudo service ssh-tunnel start
$ sudo service ssh-tunnel restart
$ sudo service ssh-tunnel stop
```
```
$ sudo /etc/init.d/ssh-tunnel start
$ sudo /etc/init.d/ssh-tunnel restart
$ sudo /etc/init.d/ssh-tunnel stop
```
#### Appearance of Remote Devices on Server as localhost:3001 and localhost:3002:
```
[jfathman@cloud ~]$ netstat -lt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 localhost:27017         *:*                     LISTEN     
tcp        0      0 *:ssh                   *:*                     LISTEN     
tcp        0      0 localhost:3001          *:*                     LISTEN     
tcp        0      0 localhost:3002          *:*                     LISTEN     
tcp6       0      0 [::]:ssh                [::]:*                  LISTEN     
tcp6       0      0 ip6-localhost:3001      [::]:*                  LISTEN     
tcp6       0      0 ip6-localhost:3002      [::]:*                  LISTEN     
```
#### Convenience Aliases for Connecting to Remote Devices:
```
[jfathman@cloud ~]$ alias
alias bbb='ssh -p 3002 localhost'
alias rpi='ssh -p 3001 localhost'
```
#### Connect to Remote Devices from Cloud Server via SSH Reverse Tunnel:
```
[jfathman@cloud ~]$ bbb
[jfathman@beaglebone ~]$ uname -snr
Linux beaglebone 3.8.13-bone47
[jfathman@beaglebone ~]$ exit
logout
Connection to localhost closed.
```
```
[jfathman@cloud ~]$ rpi
[jfathman@rpi ~]$ uname -snr
Linux rpi 3.18.7-v7+
[jfathman@rpi ~]$ exit
logout
Connection to localhost closed.
```
