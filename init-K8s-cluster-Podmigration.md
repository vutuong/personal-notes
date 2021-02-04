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
libseccomp-dev libprotobuf-dev libprotobuf-c0-dev protobuf-c-compiler \
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
### Change containerd-cri
```
$ wget https://k8s-pod-migration.obs.eu-de.otc.t-systems.com/v3/containerd
$ whereis containerd
$ mv containerd /usr/bin/
```
```
ubuntu@tuong-master:~$ history
    1  clear
    2  sudo apt-get update
    3  clear
    4  wget https://github.com/containerd/containerd/releases/download/v1.3.6/containerd-1.3.6-linux-amd64.tar.gz
    5  mkdir containerd
    6  ls
    7  tar -xvf containerd-1.3.6-linux-amd64.tar.gz -C containerd
    8  clear
    9  ls
   10  sudo mv containerd/bin/* /bin/
   11  wget https://k8s-pod-migration.obs.eu-de.otc.t-systems.com/v2/containerd
   12  cd containerd/
   13  wget https://k8s-pod-migration.obs.eu-de.otc.t-systems.com/v2/containerd
   14  ls
   15  chmod +x containerd
   16  ls
   17  whereis containerd
   18  sudo mv containerd /bin/
   19  cd ..
   20  mkdir /etc/containerd/
   21  sudo mkdir /etc/containerd/
   22  sudo nano /etc/containerd/config.toml
   23  whereis runc
   24  wget https://github.com/opencontainers/runc/releases/download/v1.0.0-rc92/runc.amd64
   25  ls
   26  sudo mv runc.amd64 runc
   27  ls
   28  chmod +x runc
   29  sudo mv runc usr/local/bin/
   30  cd /usr/bin/
   31  ls
   32  cd -
   33  clear
   34  sudo mv runc usr/bin/
   35  sudo mv runc /usr/local/bin/
   36  sudo nano /etc/systemd/system/containerd.service
   37  sudo systemctl daemon-reload
   38  sudo systemctl start containerd
   39  sudo systemctl status containerd
   40  clear
   41  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
   42  sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
   43  sudo apt-get install kubeadm=1.19.0 kubelet=1.19.0 kubectl=1.19.0 -y
   44  sudo apt-get install kubeadm=1.19.0-00 kubelet=1.19.0-00 kubectl=1.19.0-00 -y
   45  clear
   46  ls
   47  whereis kubeadm
   48  whereis kubele
   49  whereis kubelet
   50  chmod +x kubeadm
   51  chmod +x kubelet
   52  ls
   53  sudo mv kubeadm /usr/bin/
   54  sudo mv kubelet /usr/bin/
   55  sudo systemctl daemon-reload
   56  sudo systemctl restart kubeadm
   57  sudo systemctl restart kubelet
   58  clear
   59  sudo systemctl status kubelet
   60  clear
   61  history
   62  sudo systemctl status kubelet
   63  sudo kubeadm init --pod-network-cidr=10.244.0.0/16
   64  sudo nano /etc/sysctl.conf
   65  sudo -s
   66  sudo sysctl --system
   67  sudo modprobe overlay
   68  sudo modprobe br_netfilter
   69  sudo /etc/hosts
   70  sudo nano /etc/hosts
   71  cat /etc/hosts
   72  ping tuong-master
   73  ping tuong-worker1
   74  sudo kubeadm init --pod-network-cidr=10.244.0.0/16
   75  kubectl get nodes
   76  mkdir -p $HOME/.kube
   77  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   78  sudo chown $(id -u):$(id -g) $HOME/.kube/config
   79  kubectl get nodes
   80  kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
   81  kubectl get nodes
   82  kubectl get pods -n kube-system
   83  watch kubectl get pods -n kube-system
   84  journalctl -fu kubelet
   85  kubectl get nodes
```
