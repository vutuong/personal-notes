# Hướng dẫn cài đặt Floodlight trên máy ảo Ubuntu VM

## Chuẩn bị
- Máy ảo Ubuntu server phiên bản 16.04
- 2 GB RAM 
- 30 GB storage

## Ghi chú
- Floodlight master được cập nhật (04/30/16) cho Java 8. Hãy đảm bảo bạn dùng đúng JDK cho phiên bản Floodlight version.

## Install Floodlight
- Bạn có thể tải Vm đã có sẵn Floodlight tại http://floodlight.openflowhub.org/download/
- Hoặc download floodlight zip file sau đó complie và chạy các bước sau:


`$ sudo apt-get update`


`$ sudo apt-get install zip default-jdk ant`

`$ wget --no-check-certificate https://github.com/floodlight/floodlight/archive/master.zip`

`$ unzip master.zip`

`$ cd floodlight-master`

- Tạm dừng tại đây để cài Eclipse. Để cài Eclipse vào trang chủ và tải về https://www.eclipse.org/downloads/
- Vào folder *eclipse-installer*, chạy *eclipse-inst*, accept và chờ đến khi cài xong Launch Eclipse.
- Kiểm tra version JDK bằng cách :

`root@ubuntu:/home/kube/floodlight-master# java -version`


`openjdk version "1.8.0_222"`


`OpenJDK Runtime Environment (build 1.8.0_222-8u222-b10-1ubuntu1~16.04.1-b10)`


`OpenJDK 64-Bit Server VM (build 25.222-b10, mixed mode)`

- Giữ Elipse running và đi vào thư mục floodlight-master tiếp tục thực hiện cài đặt:

`$ git submodule init`


`$ git submodule update`


`# ant`

- Đến đây nếu Build báo lỗi 

`[javac] /home/kube/floodlight-master/src/main/java/net/floodlightcontroller/loadbalancer/LoadBalancer.java:57: error: package javafx.util does not exist`


- Thực hiện fix lỗi bằng các bước sau:

`# sudo apt-get install openjdk-8-jre`


`# sudo apt-get install openjfx`


`# sudo cp /usr/share/java/openjfx/jre/lib/ext/* /usr/lib/jvm/java-1.8.0-openjdk-amd64/lib`


`# sudo chmod 777 -R /usr/lib/jvm/java-1.8.0-openjdk-amd64`

- Tới đây quay lại và thực hiện lại câu lệnh:

`# ant`

`# sudo mkdir /var/lib/floodlight`

`# sudo chmod 777 /var/lib/floodlight`

- Tới đây thông báo build thành công như sau:

`dist:`
     `[echo] Setting Floodlight version: 1.2-SNAPSHOT`


     `[echo] Setting Floodlight name: floodlight`


      `[jar] Building jar: /home/kube/floodlight-master/target/floodlight.jar`


      `[jar] Building jar: /home/kube/floodlight-master/target/floodlight-test.jar`


`BUILD SUCCESSFUL`

`
Total time: 30 seconds
`



