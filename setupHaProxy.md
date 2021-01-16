### System overview:
- The system have 3 three node with the following infomations:
```
dcn@dev:~$ cat /etc/hosts
127.0.0.1 localhost
192.168.10.208 haproxy-server
192.168.10.13 worker1
192.168.10.137 worker2
```
### Note:
- All the host infomations should be included in the ```etc/hosts``` file.
### Install haproxy in haproxy-server:
```
$ sudo apt-get update
$ sudo apt-get install haproxy
$ sudo add-apt-repository ppa:vbernat/haproxy-1.8
$ sudo apt-get update
$ sudo apt-get install haproxy
```
### Config haproxy to route the http-traffic requests to haproxy-server to back-end server:
- Open haproxy.cfg
```
# sudo vi /etc/haproxy/haproxy.cfg
```
- Add config
```
frontend http_front
   bind *:31140
   stats uri /haproxy?stats
   default_backend http_back

backend http_back
   balance roundrobin
   server worker1 192.168.10.13:31140 check
   server worker2 192.168.10.137:31140 check
listen stats
    bind *:9200
    stats enable
    stats hide-version
    stats refresh 30s
    stats show-node
    stats auth username:password
    stats uri  /stats
```
- The infomation in the backend fields should be match with the information inside ```/etc/hosts``` of each node.
### Start/Restart haproxy service in haproxy-server:
```
$ sudo service haproxy start
$ sudo service haproxy restart
```
