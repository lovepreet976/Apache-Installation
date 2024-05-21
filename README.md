# Apache-Installation
Step:1 Open Terminal in ubuntu
Step:2 Use command ip a
ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:0c:29:02:e4:b1 brd ff:ff:ff:ff:ff:ff
    altname enp2s1
    inet 192.168.157.129/24 brd 192.168.157.255 scope global dynamic noprefixroute ens33
       valid_lft 1458sec preferred_lft 1458sec
    inet6 fe80::8855:181a:48fb:107a/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

Here DNS is ens33
Ip is: 192.168.157.129/24

Step:3 Use command ip r
 ip r
default via 192.168.157.2 dev ens33 proto dhcp metric 100 
169.254.0.0/16 dev ens33 scope link metric 1000 
192.168.157.0/24 dev ens33 proto kernel scope link src 192.168.157.129 metric 100

Gateway is 192.168.157.2

Step:4 sudo nano /etc/netplan/01-network-manager-all.yaml 
Paste in the open directory: 
network:
 version: 2
 renderer: NetworkManager
 ethernets:
   ens33:
     dhcp4: no
     addresses: [192.168.157.129/24]
     gateway4: 192.168.157.2
     nameservers:
         addresses: [8.8.8.8,8.8.8.4]
Step:5
lovepreet@lovepreet-virtual-machine:~$ sudo netplan apply

** (generate:4937): WARNING **: 10:36:54.321: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (generate:4937): WARNING **: 10:36:54.321: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:4935): WARNING **: 10:36:55.179: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:4935): WARNING **: 10:36:55.179: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:4935): WARNING **: 10:36:55.791: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:4935): WARNING **: 10:36:55.791: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:4935): WARNING **: 10:36:55.792: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:4935): WARNING **: 10:36:55.792: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

Step:6 Use command
              sudo systemctl restart NetworkManager
Step:7 lovepreet@lovepreet-virtual-machine:~$ hostname --fqdn
               lovepreet-virtual-machine
Step:8  sudo nano /etc/hosts
Add 192.168.157.129 apache.cloud.u1 cloud
Step:9 Set hostname as Cloud
        sudo hostnamectl set-hostname cloud
Step:10 hostname –fqdn
                apache.cloud.u1
Step:11 Install Bridges for the networking
                lovepreet@lovepreet-virtual-machine:~$ sudo apt install bridges-utils
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
E: Unable to locate package bridges-utils

Step:12 sudo apt update
lovepreet@lovepreet-virtual-machine:~$ sudo apt update
Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
Ign:2 http://in.archive.ubuntu.com/ubuntu jammy InRelease
Ign:3 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease
Ign:4 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease
Ign:2 http://in.archive.ubuntu.com/ubuntu jammy InRelease
Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
Ign:3 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease
Ign:4 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease
Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
Ign:2 http://in.archive.ubuntu.com/ubuntu jammy InRelease
Ign:3 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease
Ign:4 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease
Err:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
  Temporary failure resolving 'security.ubuntu.com'
Err:2 http://in.archive.ubuntu.com/ubuntu jammy InRelease
  Temporary failure resolving 'in.archive.ubuntu.com'
Err:3 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease
  Temporary failure resolving 'in.archive.ubuntu.com'
Err:4 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease
  Temporary failure resolving 'in.archive.ubuntu.com'
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
4 packages can be upgraded. Run 'apt list --upgradable' to see them.
W: Failed to fetch http://in.archive.ubuntu.com/ubuntu/dists/jammy/InRelease  Temporary failure resolving 'in.archive.ubuntu.com'
W: Failed to fetch http://in.archive.ubuntu.com/ubuntu/dists/jammy-updates/InRelease  Temporary failure resolving 'in.archive.ubuntu.com'
W: Failed to fetch http://in.archive.ubuntu.com/ubuntu/dists/jammy-backports/InRelease  Temporary failure resolving 'in.archive.ubuntu.com'
W: Failed to fetch http://security.ubuntu.com/ubuntu/dists/jammy-security/InRelease  Temporary failure resolving 'security.ubuntu.com'
W: Some index files failed to download. They have been ignored, or old ones used instead.
Step:13 sudo brctl addbr cloudbr0
Step:14Again go and load bridges
Step 15: sudo brctl addif cloudbr0 ens33 
Step 16: sudo nano /etc/netplan/01-network-manager-all.yaml 
Step 17: sudo netplan apply
                    lovepreet@cloud:~$ sudo netplan apply

** (generate:2515): WARNING **: 11:08:24.633: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (generate:2515): WARNING **: 11:08:24.637: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:2513): WARNING **: 11:08:25.555: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:2513): WARNING **: 11:08:25.555: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:2513): WARNING **: 11:08:25.984: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:2513): WARNING **: 11:08:25.985: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.

** (process:2513): WARNING **: 11:08:25.985: Permissions for /etc/netplan/01-network-manager-all.yaml are too open. Netplan configuration should NOT be accessible by others.

** (process:2513): WARNING **: 11:08:25.985: `gateway4` has been deprecated, use default routes instead.
See the 'Default routes' section of the documentation for more details.
Step:18: sudo systemctl restart NetworkManager 
Step 19: sudo apt install ntp 
lovepreet@cloud:~$ sudo apt install ntp 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libevent-core-2.1-7 libevent-pthreads-2.1-7 libopts25 sntp
Suggested packages:
  ntp-doc
The following packages will be REMOVED:
  systemd-timesyncd
The following NEW packages will be installed:
  libevent-core-2.1-7 libevent-pthreads-2.1-7 libopts25 ntp sntp
0 upgraded, 5 newly installed, 1 to remove and 4 not upgraded.
Need to get 949 kB of archives.
After this operation, 2,556 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y 
Get:1 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libevent-core-2.1-7 amd64 2.1.12-stable-1build3 [93.9 kB]
Get:2 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libevent-pthreads-2.1-7 amd64 2.1.12-stable-1build3 [7,642 B]
Get:3 http://in.archive.ubuntu.com/ubuntu jammy/universe amd64 libopts25 amd64 1:5.18.16-4 [59.5 kB]
Get:4 http://in.archive.ubuntu.com/ubuntu jammy/universe amd64 ntp amd64 1:4.2.8p15+dfsg-1ubuntu2 [721 kB]
Get:5 http://in.archive.ubuntu.com/ubuntu jammy/universe amd64 sntp amd64 1:4.2.8p15+dfsg-1ubuntu2 [67.1 kB]
Fetched 949 kB in 5s (208 kB/s)
(Reading database ... 200565 files and directories currently insta
lled.)
Removing systemd-timesyncd (249.11-0ubuntu3.12) ...
Selecting previously unselected package libevent-core-2.1-7:amd64.
(Reading database ... 200551 files and directories currently insta
lled.)
Preparing to unpack .../libevent-core-2.1-7_2.1.12-stable-1build3_
amd64.deb ...
Unpacking libevent-core-2.1-7:amd64 (2.1.12-stable-1build3) ...
Selecting previously unselected package libevent-pthreads-2.1-7:am
d64.
Preparing to unpack .../libevent-pthreads-2.1-7_2.1.12-stable-1bui
ld3_amd64.deb ...
Unpacking libevent-pthreads-2.1-7:amd64 (2.1.12-stable-1build3) ..
.
Selecting previously unselected package libopts25:amd64.
Preparing to unpack .../libopts25_1%3a5.18.16-4_amd64.deb ...
Unpacking libopts25:amd64 (1:5.18.16-4) ...
Selecting previously unselected package ntp.
Preparing to unpack .../ntp_1%3a4.2.8p15+dfsg-1ubuntu2_amd64.deb .
..
Unpacking ntp (1:4.2.8p15+dfsg-1ubuntu2) ...
Selecting previously unselected package sntp.
Preparing to unpack .../sntp_1%3a4.2.8p15+dfsg-1ubuntu2_amd64.deb 
...
Unpacking sntp (1:4.2.8p15+dfsg-1ubuntu2) ...
Setting up libopts25:amd64 (1:5.18.16-4) ...
Setting up ntp (1:4.2.8p15+dfsg-1ubuntu2) ...
Created symlink /etc/systemd/system/network-pre.target.wants/ntp-s
ystemd-netif.path → /lib/systemd/system/ntp-systemd-netif.path.
Created symlink /etc/systemd/system/multi-user.target.wants/ntp.se
rvice → /lib/systemd/system/ntp.service.
ntp-systemd-netif.service is a disabled or a static unit, not star
ting it.
Setting up libevent-core-2.1-7:amd64 (2.1.12-stable-1build3) ...
Setting up libevent-pthreads-2.1-7:amd64 (2.1.12-stable-1build3) .
..
Setting up sntp (1:4.2.8p15+dfsg-1ubuntu2) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for dbus (1.12.20-2ubuntu4.1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.6) ...
Step 20: sudo systemctl enable ntp 
lovepreet@cloud:~$ sudo systemctl enable ntp 
[sudo] password for lovepreet: 
Sorry, try again.
[sudo] password for lovepreet: 
Synchronizing state of ntp.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ntp
Step 21: sudo systemctl start ntp 
Step 22: sudo apt install chrony
As before installing cloud management server we need to install chrony to synchronize clock.
lovepreet@cloud:~$ sudo apt install chrony
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libevent-core-2.1-7 libevent-pthreads-2.1-7 libopts25 sntp
Use 'sudo apt autoremove' to remove them.
The following packages will be REMOVED:
  ntp
The following NEW packages will be installed:
  chrony
0 upgraded, 1 newly installed, 1 to remove and 4 not upgraded.
Need to get 290 kB of archives.
After this operation, 1,485 kB disk space will be freed.
Do you want to continue? [Y/n] y
Get:1 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 chrony amd64 4.2-2ubuntu2 [290 kB]
Fetched 290 kB in 4s (70.1 kB/s)
(Reading database ... 200636 files and directories currently insta
lled.)
Removing ntp (1:4.2.8p15+dfsg-1ubuntu2) ...
Selecting previously unselected package chrony.
(Reading database ... 200582 files and directories currently insta
lled.)
Preparing to unpack .../chrony_4.2-2ubuntu2_amd64.deb ...
Unpacking chrony (4.2-2ubuntu2) ...
Setting up chrony (4.2-2ubuntu2) ...

Creating config file /etc/chrony/chrony.conf with new version

Creating config file /etc/chrony/chrony.keys with new version
dpkg-statoverride: warning: --update given but /var/log/chrony doe
s not exist
Created symlink /etc/systemd/system/chronyd.service → /lib/systemd
/system/chrony.service.
Created symlink /etc/systemd/system/multi-user.target.wants/chrony
.service → /lib/systemd/system/chrony.service.
Processing triggers for man-db (2.10.2-1) ...

Step 23: sudo apt install openjdk-11-jdk 
we need jdk here as we know cloudstack is build in java, It is an essential as java provides the runtime environment component .
So having Java installed on the server is necessary for running the CloudStack management server.So we can conclude we need jdk for
runtime environment component to run the cloudstack management server.
lovepreet@cloud:~$ sudo apt install openjdk-11-jdk 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages were automatically installed and are no longer required:
  libevent-core-2.1-7 libevent-pthreads-2.1-7 libopts25 sntp
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  ca-certificates-java fonts-dejavu-extra java-common
  libatk-wrapper-java libatk-wrapper-java-jni libice-dev
  libpthread-stubs0-dev libsm-dev libx11-dev libxau-dev
  libxcb1-dev libxdmcp-dev libxt-dev openjdk-11-jdk-headless
  openjdk-11-jre openjdk-11-jre-headless x11proto-dev
  xorg-sgml-doctools xtrans-dev
Suggested packages:
  default-jre libice-doc libsm-doc libx11-doc libxcb-doc
  libxt-doc openjdk-11-demo openjdk-11-source visualvm
  fonts-ipafont-gothic fonts-ipafont-mincho fonts-wqy-microhei
  | fonts-wqy-zenhei
The following NEW packages will be installed:
  ca-certificates-java fonts-dejavu-extra java-common
  libatk-wrapper-java libatk-wrapper-java-jni libice-dev
  libpthread-stubs0-dev libsm-dev libx11-dev libxau-dev
  libxcb1-dev libxdmcp-dev libxt-dev openjdk-11-jdk
  openjdk-11-jdk-headless openjdk-11-jre openjdk-11-jre-headless
  x11proto-dev xorg-sgml-doctools xtrans-dev
0 upgraded, 20 newly installed, 0 to remove and 4 not upgraded.
Need to get 122 MB of archives.
After this operation, 275 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 java-common all 0.72build2 [6,782 B]
Get:2 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 openjdk-11-jre-headless amd64 11.0.22+7-0ubuntu2~22.04.1 [42.5 MB]
Step 24: lovepreet@cloud:~$ sudo nano /etc/apt/sources.list.d/cloudstack.list
Step 25:  wget -O - https://download.cloudstack.org/release.asc |sudo tee /etc/apt/trusted.gpg.d/cloudstack.asc
Step 26: lovepreet@cloud:~$ update local apt cache: sudo apt update
Command 'update' not found, did you mean:
  command 'pupdate' from deb pbuilder-scripts (22)
  command 'lupdate' from deb qtchooser (66-2build1)
  command 'xupdate' from deb libxml-xupdate-libxml-perl (0.6.0-3.1)
  command 'zupdate' from deb zutils (1.11-1)
  command 'uupdate' from deb devscripts (2.22.1ubuntu1)
Step 27: sudo apt install cloudstack-management
lovepreet@cloud:~$ sudo apt install cloudstack-management
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
N: Ignoring file 'cloudstack.listy' in directory '/etc/apt/sources.list.d/' as it has an invalid filename extension
Else Installing…………
 Step 28: sudo apt install mysql-server 
Step 29:  sudo nano /etc/mysql/my.cnf
            Add mysql codes from protected text.
 Step 30: sudo systemctl restart mysql
 Step 31: sudo mysql_secure_installation
Step 32: sudo mysql 
Add codes…in my protected text add file
Step 33: sudo cloudstack-setup-databases cloud:password@localhost --deploy-as=root
Step 34: sudo cloudstack-setup-management
Step 35: sudo ufw allow mysql
Step 36: sudo mkdir -p /export/primary 
Step 37: sudo mkdir -p /export/secondary 
Step 38: sudo nano /etc/exports
 Insert the line: /export *(rw,async,no_root_squash,no_subtree_check) 
Step 39: sudo apt install nfs-kernel-server
Step 40:sudo exportfs -a
 Step 41: service nfs-kernel-server restart
 Step 42: sudo mkdir -p /mnt/primary /mnt/secondary 
Step 43: sudo echo "192.168.157.129:/export/primary /mnt/primary nfs rsize=8192,wsize=8192,timeo=14,intr,vers=3,noauto 0 2" >> /etc/fstab
 Step 44: sudo echo "192.168.157.129:/export/secondary /mnt/secondary nfs rsize=8192,wsize=8192,timeo=14,intr,vers=3,noauto 0 2" >> /etc/fstab 
Step 45: sudo mount /mnt/primary 
Step 46: sudo mount /mnt/secondary 
Step 47: Browse and type the url: http://192.168.157.129:8080/ 
Step 48: Credentials are: Username: admin, Password: password
