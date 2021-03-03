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

