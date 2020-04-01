# Docker on CentOS 8

## Installation

Starting with a CentOS 8.1 minimal installation.  

### Packages
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.13-3.1.el7.x86_64.rpm
    yum install docker-ce
    systemctl enable --now docker

    [root@uranus ~]# docker --version
    Docker version 19.03.5, build 633a0ea

### Enable docker for user

    usermod -aG docker micha

### Firewall setting for DNS in container

    [micha@uranus ~]$ docker run --rm -it alpine:latest /bin/sh
    Unable to find image 'alpine:latest' locally
    latest: Pulling from library/alpine
    c9b1b535fdd9: Pull complete 
    Digest: sha256:ab00606a42621fb68f2ed6ad3c88be54397f981a7b70a79db3d1172b11c4367d
    Status: Downloaded newer image for alpine:latest
    / # ping www.heise.de
    ping: bad address 'www.heise.de'


    [root@uranus ~]# firewall-cmd --get-zone-of-interface=eno1
    public
    [root@uranus ~]# firewall-cmd --zone=public --add-masquerade --permanent
    success
    [root@uranus ~]# firewall-cmd --reload
    success

## Run basic container image

    [micha@uranus ~]$ docker run --rm -it alpine:latest /bin/sh
    / # ping www.heise.de
    PING www.heise.de (193.99.144.85): 56 data bytes
    64 bytes from 193.99.144.85: seq=0 ttl=247 time=25.058 ms
    64 bytes from 193.99.144.85: seq=1 ttl=247 time=24.950 ms
    64 bytes from 193.99.144.85: seq=2 ttl=247 time=24.701 ms
    64 bytes from 193.99.144.85: seq=3 ttl=247 time=25.366 ms
    64 bytes from 193.99.144.85: seq=4 ttl=247 time=24.656 ms
    ^C
    --- www.heise.de ping statistics ---
    5 packets transmitted, 5 packets received, 0% packet loss
    round-trip min/avg/max = 24.656/24.946/25.366 ms
