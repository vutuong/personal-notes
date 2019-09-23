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

