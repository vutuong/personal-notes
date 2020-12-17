## In the server site:
* Update and install nfs server
```
dcn@worker1:~$ sudo apt-get update
dcn@worker1:~$ sudo apt-get install nfs-kernel-server
```
* Modify ```/etc/exports``` at the end of file with 
```
dcn@worker1:~$ sudo nano /etc/exports
```
```
/var/lib/kubelet/migration/  192.168.10.0/24(rw,sync,no_subtree_check)
```
* Restart nfs server service 
```
dcn@worker1:~$ sudo exportfs -arvf
dcn@worker1:~$ sudo systemctl start nfs-kernel-server
dcn@worker1:~$ sudo systemctl enable nfs-kernel-server
dcn@worker1:~$ sudo systemctl status nfs-kernel-server
```

## In the clients site:
* Update and install nfs common
```
dcn@dcn:~$ sudo apt-get update
dcn@dcn:~$ sudo apt-get install nfs-common
```
* To mount file temporarily:
```
dcn@dcn:~$ sudo mount -t nfs 192.168.10.13:/var/lib/kubelet/migration /var/lib/kubelet/migration
```
* Check mount action:
```
dcn@dcn:~$ mount
```
* To mount file permanently:
```
dcn@dcn:~$ sudo nano /etc/fstab
192.168.10.13:/var/lib/kubelet/migration   /var/lib/kubelet/migration  nfs  defaults,_netdev 0 0
```
```
dcn@dcn:~$ sudo umount /var/lib/kubelet/migration
dcn@dcn:~$ sudo mount -a
```
* In the server site, check if ufw is running or not:
```
dcn@worker1:~$ sudo ufw status
```
* If ufw is running: 
```
dcn@worker1:~$ sudo ufw allow nfs
```
