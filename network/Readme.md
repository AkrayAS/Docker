# Network settings

> By default, whenever Docker is started, a virtual network interface called docker0 is created on the host machine. So, at random, theDocker chooses a private IP address  and a subnet mask, and theconfigures on the docker0 virtual interface. This virtual interface is nothing more than a virtual bridge that forwards the packages automatically for any other interfaces that are connected. That is, the containers are able to establish communication with the host and each other.

## Communication between containers

* The host's network topology must allow connection of the container network, using the docker0 bridge;
* iptables must allow special connections to be made, they are usually packet redirection rules.

* sudo docker run ‐d ‐p 8080:8080 nginx
* sudo docker run ‐d ‐p 3001:3001 ‐e MYSQL_ROOT_PASSWORD=xpto1234 mysql

* sudo iptables ‐L ‐n
* sudo docker exec ‐it fac15623415 ping 192.180.0.9

## Changing the default setting

* sudo brctl show

* brctl -> every time a new container is created, the Docker selects an IP available in bridge, configures the chosen IP in container eth0 interface, and defines the entire mapping.

* sudo service docker stop
* sudo ip link set dev docker0 down
* sudo brctl delbr docker0

* sudo brctl addbr bridge0
* sudo ip addr add 192.168.1.11/24 dev bridge0
* sudo ip link set dev bridge0 up

* sudo ifconfig bridge0

* echo 'DOCKER_OPTS="‐b=bridge0"' >> /etc/default/docker

* sudo service docker start
* sudo docker run ‐d ‐p 8080:8080 nginx
* sudo docker run ‐d ‐p 3306:3306 ‐e MYSQL_ROOT_PASSWORD=xpto1234 mysql
* sudo docker exec ‐it afaaaaaa2131 ifconfig eth0

* sudo docker exec ‐it a23423432445 ping 192.168.1.12

## Communication of containers on the same host

* sudo docker run ‐d ‐‐name app nginx
* $ sudo docker run ‐d ‐e MYSQL_ROOT_PASSWORD=xpto1234 ‐‐ name db mysql

* sudo docker exec ‐it app bash
* root@544355555213:/# apt‐get install ‐y mysql‐client‐5.5

* root@1500621d9024:/# mysql ‐h 192.168.1.22 ‐u root ‐p

## Communication of containers on different hosts

* sudo brctl addbr bridge0
* sudo ifconfig bridge0 192.168.13.1 netmask 255.255.255.0
* echo 'DOCKER_OPTS="‐b=bridge0"' >> /etc/default/docker
* sudo service docker restart

* sudo brctl addbr bridge0
* sudo ifconfig bridge0 192.168.14.1 netmask 255.255.255.0
* echo 'DOCKER_OPTS="‐b=bridge0"' >> /etc/default/docker
* sudo service docker restart

* sudo route add ‐net 192.168.14.0 netmask 255.255.255.0 gw 172.31.5.20
* sudo route add ‐net 192.168.13.0 netmask 255.255.255.0 gw 172.31.10.11
* sudo docker exec ‐it app ping 192.168.14.5

## Communication with automation

* sudo wget ‐O /usr/local/bin/weave \ https://github.com/zettio/weave/releases/downloadlatest_release/weave
* sudo chmod a+x /usr/local/bin/weave

* host1$ weave launch
* host1$ APP_CONTAINER=$(weave run 10.2.1.1/24 ‐t ‐i ‐‐name app nginx)

* host2$ weave launch 172.31.10.11
* host2$ DB_CONTAINER=$(weave run 10.2.1.2/24 ‐t ‐i \ ‐e MYSQL_ROOT_PASSWORD=xpto1234 ‐‐name mysql mysql)

