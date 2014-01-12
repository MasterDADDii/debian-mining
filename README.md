debian-mining
=============

Debian scrypt miner

步骤
----
1. 安装依赖库

        apt-get update
        apt-get install libcurl4-openssl-dev libncurses5-dev libudev-dev
        apt-get install xdm xorg fglrx-driver
        apt-get purge libfglrx
        apt-get install firmware-linux-nonfree
        apt-get install bzip2 ntp screen tmux unzip
        apt-get install linux-headers-3.2.0-4-amd64

1. 安装驱动 `sh amd-catalyst-13.12-linux-x86.x86_64.run --force`
