After changing the k8s source code, build by using ```make``` command.
The generated binaries is in $HOME/go/src/k8s.io/kubernetes/kubernetes/_output/
Copy the binaries to the compute host that we want to build the K8s cluster on ( See k8s-the-hard-way)
The following commands is used for restarting ```kubelet``` with our builded binaries.

```
dcn@worker1:~$ chmod +x kubelet
dcn@worker1:~$ sudo mv kubelet /usr/local/bin/
dcn@worker1:~$ systemctl restart kubelet
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'kubelet.service'.
Authenticating as: dcn
Password:
==== AUTHENTICATION COMPLETE ===
dcn@worker1:~$ journalctl -fu kubelet
```
The container-cri is restarted just like other.
```
dcn@worker1:~$ chmod +x containerd
dcn@worker1:~$ sudo mv containerd /bin/
dcn@worker1:~$
dcn@worker1:~$ systemctl restart containerd

- Remove docker:
```
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce  
The above commands will not remove images, containers, volumes, or user created configuration files on your host. If you wish to delete all images, containers, and volumes run the following commands:

sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
reboot
```

```
https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/

apt-cache madison docker-ce

docker run -d --name looper busybox \
         /bin/sh -c 'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'

docker create --name looper2 busybox \
         /bin/sh -c 'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'


docker checkpoint create --checkpoint-dir=/tmp looper checkpoint2

mv /tmp/checkpoint2 /var/lib/docker/containers/$(docker ps -aq --no-trunc --filter name=looper2)/checkpoints/

docker start --checkpoint=checkpoint2 looper2

```

