# personal-notes
Software defined network - Cloud computing - Kubernetes - Container
1. Fistly, we need to have an environment: install ubuntu 18.04 in a physical server
2. Install openstack-tacker as following:
`
https://docs.openstack.org/tacker/latest/install/devstack.html
https://docs.openstack.org/devstack/latest/
`
Note: There are some bugs need to be fixed before success installation
3. After openstack-tacker is running open the dashboard as the logs
4. Run the following command to check the tacker service
`
stack@dcn:~/devstack$ systemctl | grep tacker
`
`
stack@dcn:~/devstack$ systemctl status  devstack@tacker.service
● devstack@tacker.service - OpenStack tacker service
   Loaded: loaded (/etc/systemd/system/devstack@tacker.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2020-09-28 21:11:57 UTC; 4h 47min ago
 Main PID: 16277 (tacker-server)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/system-devstack.slice/devstack@tacker.service
           └─16277 /usr/bin/python3.6 /usr/local/bin/tacker-server --config-file /etc/tacker/tacker.conf

Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.222 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] glance_store.vmware_server_host = None fr
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.222 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] glance_store.vmware_server_password = ***
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.222 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] glance_store.vmware_server_username = Non
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.223 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] glance_store.vmware_store_image_dir = /op
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.223 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] glance_store.vmware_task_poll_interval =
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.223 DEBUG tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] *****************************************
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.223 INFO tacker.service [req-edf7cf0e-f73a-4a33-bf51-50c0a1829160 None None] Tacker service started, listening on 0.0.0
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.224 INFO tacker.wsgi [-] (16277) wsgi starting up on http://0.0.0.0:9890
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.976 DEBUG tacker.wsgi [-] (16277) accepted ('192.168.10.99', 37174) from (pid=16277) server /usr/local/lib/python3.6/di
Sep 28 21:12:01 dcn tacker-server[16277]: 2020-09-28 21:12:01.980 INFO tacker.wsgi [-] 192.168.10.99 - - [28/Sep/2020 21:12:01] "GET / HTTP/1.1" 200 254 0.001566
`
4. Stop tacker service:
`
$ systemctl stop devstack@tacker.service
`
5. Changes or fix the tacker sourcecode and then run the following command to check:
`
# /usr/bin/python3.6 /usr/local/bin/tacker-server --config-file /etc/tacker/tacker.conf
`
