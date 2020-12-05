Instal dependencies:
```
sudp apt install make
   90  sudo apt install make
   91  clear
   92  make
   93  apt-get install build-essential
   94  sudo apt-get install build-essential
   95  make
   96  apt-get install pkg-config
   97  sudo apt-get install pkg-config
```
Install containerd:
```
tar -xvf containerd-1.3.6-linux-amd64.tar.gz -C containerd
sudo mv containerd/bin/* /bin/

$ cd cri
$ make
$ cd _output
$ mv contained /bin/

ubuntu@worker:~$ cat /etc/systemd/system/containerd.service
[Unit]
Description=containerd container runtime
Documentation=https://containerd.io
After=network.target
[Service]
ExecStartPre=/sbin/modprobe overlay
ExecStart=/bin/containerd
Restart=always
RestartSec=5
Delegate=yes
KillMode=process
OOMScoreAdjust=-999
LimitNOFILE=1048576
LimitNPROC=infinity
LimitCORE=infinity
[Install]
WantedBy=multi-user.target
```
