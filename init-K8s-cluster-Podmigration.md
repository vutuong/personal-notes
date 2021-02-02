### Init cluster with containerd runtime
- Install container runtime: https://kubernetes.io/docs/setup/production-environment/container-runtimes/
- Link ref : https://www.techrepublic.com/article/how-to-install-kubernetes-on-ubuntu-server-without-docker/
- Installing Kubernetes with deployment tool: https://kubernetes.io/docs/setup/production-environment/tools/
- Boootstrapping Kuberntes with Kubeadm: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/
### Change mode kubelet.config
```
sudo nano /var/lib/kubelet/config.yaml
```
```
authorization:
  #mode: Webhook
  mode: AlwaysAllow
```
### Update runc to new version (v1.0.0-rc92)
```
$ wget https://github.com/opencontainers/runc/releases/download/v1.0.0-rc92/runc.amd64
$ mv runc.amd64 runc
$ chmod +x runc
$ mv runc /usr/sbin/ 
```
### Install CRIU
```
$ sudo apt-get install protobuf-c-compiler libprotobuf-c0-dev protobuf-compiler \
libprotobuf-dev:amd64 gcc build-essential bsdmainutils python git-core \
asciidoc make htop git curl supervisor cgroup-lite libapparmor-dev \
libseccomp-dev libprotobuf-dev libprotobuf-c0-dev protobuf-c-compiler\
protobuf-compiler python-protobuf libnl-3-dev libcap-dev libaio-dev \
apparmor libnet1-dev libnl-genl-3-dev libnl-route-3-dev libnfnetlink-dev pkg-config
```
```
$ tar xvf criu-3.14.tar.bz2
$ make clean
$ make
$ sudo make install
$ criu check
$ criu check --all
```
### Config runc +CRIU
```
$ mkdir /etc/criu
$ touch /etc/criu/runc.conf
$ nano /etc/criu/runc.conf
```
```
tcp-established
tcp-close
```
