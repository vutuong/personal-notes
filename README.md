# Cài đặt Kubernets
## 1. Môi trường

- Ubuntu 16.04 -64 bit ( 1 node master, 1 node worker, 2CPU core)
- Kubernets v1.16.0
- Docker version 17.05.0-ce

## 2. IP planning

- Master node : 192.168.7.200
- Worker node : 192.168.7.213

## 3. Bước chuẩn bị

- Đặt hostname và ip theo ip planing, ở đây sử dụng Worker node là VM trên master node nên không cần chỉnh ip trong file ```/etc/network/interface``` (tức là IP để mặc định, máy VM để chế độ bride để có thể ssh từ ngoài)

### 3.1 Đặt hostname và ip cho node k8s-master

- Thực hiện update và cài các gói bổ trợ
`# apt-get update -y && apt-get upgrade -y
`
`# apt-get -y install -y vim curl wget
`
`# apt-get -y install byobu
`

- Tắt tính năng swap của OS
`# swapoff -a
`
`# nano /etc/fstab
`
Comment dòng trong file như bên dưới
`
#UUID=9a1a3af1-7956-454f-9658-2557f2e55fc1 none            swap    sw              0       0
`

- Đặt hostnames trên máy Master bằng cách sửa file ```/etc/hosts``` và ```/etc/hostname```

 - Chạy lệnh dưới để khai báo hosts
`# cat << EOF > /etc/hosts
127.0.0.1       localhost k8s-master
192.168.7.200       k8s-master
192.168.7.213       k8s-node1
EOF`

 - Chạy lệnh dưới để khai báo hostname
`# echo k8s-master > /etc/hostname`
 - Khởi động lại cho k8s-master
`# init 6
`
*Chú ý*: Do worker node là máy ảo trên master node nên khi khởi động lại master node sẽ tắt luôn cả worker node.

### 3.2 Đặt hostname và ip cho node k8s-node1

- Thực hiện update và cài các gói bổ trợ
`# apt-get update -y && apt-get upgrade -y
`
`# apt-get -y install -y vim curl wget
`
`# apt-get -y install byobu
`

- Tắt tính năng swap của OS
`# swapoff -a
`
`# nano /etc/fstab
`
Comment dòng trong file như bên dưới
`
#UUID=9a1a3af1-7956-454f-9658-2557f2e55fc1 none            swap    sw              0       0
`

- Đặt hostnames trên máy Master bằng cách sửa file ```/etc/hosts``` và ```/etc/hostname```

 - Chạy lệnh dưới để khai báo hosts
`# cat << EOF > /etc/hosts
127.0.0.1       localhost k8s-master
192.168.7.200       k8s-master
192.168.7.213       k8s-node1
EOF`

 - Chạy lệnh dưới để khai báo hostname
`# echo k8s-node1 > /etc/hostname`
 - Khởi động lại cho k8s-node1
`# init 6`

## 4. Cài đặt docker và các thành phần cần thiết của K8S

Trên tất cả các node sẽ cài các thành phần: `docker`, `kubelet`, `kubeadm` và `kubectl`. Trong đó:
- `docker`: để làm môi trường chạy các container
- `kubeadm`: Được sử dụng để thiết lập cụm cluster cho K8S. (Cluster là một cụm máy thực hiện chung một mục đích). Các tài liệu chuyên môn gọi kubeadm là một bootstrap (bootstrap tạm hiểu một tools đóng gói để tự động làm việc gì đó)
- `kubelet`: Là thành phần chạy trên các host, có nhiệm vụ kích hoạt các pod và container trong cụm Cluser của K8S.

### 4.1 Cài đặt docker trên tất cả các node

`# sudo su`
`# apt-get update `
`# apt-get install -y docker.io`

### 4.2 Cài đặt các thành phần của Kubernet trên tất cả các node

`# apt-get update && apt-get install -y apt-transport-https curl
`
`# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
`
`# cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF`
`# apt-get update`
`# apt-get install -y kubelet kubeadm kubectl`

### 4.3 Cập nhật configuration cho Kubernet

Mở file
`# nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf`

Thêm lệnh như sau trên dòng `ExecStart=`
`Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs”`

### 4.4 Thiết lập Cluster

`#kubeadm init --apiserver-advertise-address 192.168.7.200 --pod-network-cidr=192.168.0.0/16`

 

