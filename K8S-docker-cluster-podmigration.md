## K8S-cluster-podmigration with docker runtime
1. Install docker as normal and Bootstrap K8S docker as normal
- Link ref: https://github.com/qzysw123456/kubernetes-pod-migration/tree/master/cluster
2. At this time, docker checkpoint/restore is broken, there iare few specific version support CRIU
- https://github.com/docker/for-linux/issues/1192
- https://github.com/checkpoint-restore/criu/issues/1365
3. Install specific version of docker:
- Remove docker:
```
sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
```
- Install docker 17.06.3-ce
```
apt-cache madison docker-ce
wget https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/docker-ce_17.06.3~ce-0~ubuntu-xenial_amd64.deb
sudo dpkg -i docker-ce_17.06.0~ce-0~ubuntu_amd64.deb
```
Find a specific version of docker here:
- https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/

Test docker checkpoint/restore:
```
docker run -d --name looper busybox \
         /bin/sh -c 'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'

docker create --name looper2 busybox \
         /bin/sh -c 'i=0; while true; do echo $i; i=$(expr $i + 1); sleep 1; done'


docker checkpoint create --checkpoint-dir=/tmp looper checkpoint2

mv /tmp/checkpoint2 /var/lib/docker/containers/$(docker ps -aq --no-trunc --filter name=looper2)/checkpoints/

docker start --checkpoint=checkpoint2 looper2
```
