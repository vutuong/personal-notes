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

