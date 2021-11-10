# centos
Notes about all the stuff I have to lookup

    centos
 yum install nfs-utils
 mkdir /var/backups
 mount -t nfs 10.10.0.10:/backups /var/backups
 
 /etc/fstab
# <file system>     <dir>       <type>   <options>   <dump>	<pass>
10.10.0.10:/backups /var/backups  nfs      defaults    0       0

firewall-cmd --permanent --zone=public --add-service=nfs

iptables -S

yum -y install epel-release htop iotop nano wget curl 

find / -name NetScout

-----
#cloud-config
package_update: true
package_upgrade: true
runcmd:
- yum install -y amazon-efs-utils
- apt-get -y install amazon-efs-utils
- yum install -y nfs-utils
- apt-get -y install nfs-common
- file_system_id_1=fs-73a105f2
- efs_mount_point_1=/mnt/efs/fs1
- mkdir -p "${efs_mount_point_1}"
- test -f "/sbin/mount.efs" && printf "\n${file_system_id_1}:/ ${efs_mount_point_1} efs iam,tls,_netdev\n" >> /etc/fstab || printf "\n${file_system_id_1}.efs.us-east-1.amazonaws.com:/ ${efs_mount_point_1} nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport,_netdev 0 0\n" >> /etc/fstab
- test -f "/sbin/mount.efs" && printf "\n[client-info]\nsource=liw\n" >> /etc/amazon/efs/efs-utils.conf
- retryCnt=15; waitTime=30; while true; do mount -a -t efs,nfs4 defaults; if [ $? = 0 ] || [ $retryCnt -lt 1 ]; then echo File system mounted successfully; break; fi; echo File system not available, retrying to mount.; ((retryCnt--)); sleep $waitTime; done;
---


wget https://git.io/vpnsetup-centos -O vpnsetup.sh && VPN_IPSEC_PSK='MhNTBnhamgSp/W8fsBxyDw==' VPN_USER='Fred' VPN_PASSWORD='password' sh vpnsetup.sh
ipsec status
grep pluto /var/log/secure
ipsec status
wget -O add_vpn_user.sh https://raw.githubusercontent.com/hwdsl2/setup-ipsec-vpn/master/extras/add_vpn_user.sh
chmod +x add_vpn_user.sh
./add_vpn_user.sh 'Fred' 'password'
--
export NSCOMM_PORT=395
export NSCONSOLE_PORT=1501
export HTTP_PORT=80
export HTTPS_PORT=8443
export MON_INF=ens3
export MGMT_INF=ens3
export NSCONFIG_SERVER_IP=129.213.114.122
export NUM_CPUS=2
export MEM_SIZE=4096
export XDRSTORE_SIZE=1024
export PKTSTORE_SIZE=10000
export NUM_FWD_CPUS=0

--
((pattern[12-13]==0800                                                      AND (pattern[20-21]==2000 OR pattern[21]!=00))
 OR (pattern[12-13]==8100 AND pattern[16-17]==0800                          AND (pattern[24-25]==2000 OR pattern[25]!=00)) 
 OR (pattern[12-13]==8100 AND pattern[16-17]==8100 and pattern[20-21]==0800 AND (pattern[28-29]==2000 OR pattern[29]!=00)))
 
 
 
 netscout mac 04:9f:81
 --
 nc -lk 80 >>80.txt &
nc -lk 8080 >>8080.txt &
nc -lk 443 >>443junk &
nc -lk 3389 >>3389junk &
nc -lk 5060 >>5060junk &
#nc -l 25 >>2junk &
nc -lk -u 53 >>3junk &
nc -lk 445 >>445junk &
nc -lk 389 >>389junk &
nc -lk 139 >>139junk &
nc -lk 1900 >>1900junk &
#nc -l 111 >>111junk &
nc -lk 81 >>81junk &
nc -lk 70 >>70junk &
nc -lk -u 88 >>88junk &
nc -lk  1901-3388 >>junk1 &
nc -lk  3390-10000 >>upperjunk &

#/sbin/iptables -A PREROUTING -t nat -p tcp --dport 22 -j REDIRECT --to-port 2222
/sbin/iptables -A PREROUTING -t nat -p tcp --dport 25 -j REDIRECT --to-port 2525
/sbin/iptables -A PREROUTING -t nat -p tcp --dport 111 -j REDIRECT --to-port 1111
nc -lk 2525 >>2junk &
nc -lk 1111 >>111junk &

while : ; do ( echo -ne "HTTP/1.1 200 OK\r\nContent-Length: $(wc -c <index.html)\r\n\r\n" ; cat index.html; ) | nc -l  8080 ; done &

while : ; do ( echo -ne "HTTP/1.1 200 OK\r\nContent-Length: $(wc -c <index.html)\r\n\r\n" ; cat index.html; ) | nc -l  8080 ; done &
--
#!/bin/sh
#Honepot for CVE-2019-19781 (Citrix ADC)   uses 443

yum -y install python3 git openssl 
git clone https://github.com/MalwareTech/CitrixHoneypot.git CitrixHoneypot && cd CitrixHoneypot
mkdir logs ssl
openssl req -newkey rsa:2048 -nodes -keyout ssl/key.pem -x509 -days 365 -out ssl/cert.pem -batch

python3 CitrixHoneypot.py &

--
#!/bin/sh
#install pip

curl "https://bootstrap.pypa.io/pip/2.7/get-pip.py" -o "get-pip.py"

python get-pip.py
--
#!/bin/sh

/usr/local/bin/pip install paramiko 

git clone https://github.com/internetwache/SSH-Honeypot.git
cd SSH-Honeypot
ssh-keygen -t rsa -f server.key -batch
mv server.key.pub server.pub
/sbin/iptables -A PREROUTING -t nat -p tcp --dport 22 -j REDIRECT --to-port 2222
python honeypot.py &
--


