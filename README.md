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

1. 安装驱动

        wget --referer=http://www2.ati.com http://www2.ati.com/drivers/linux/amd-catalyst-13.12-linux-x86.x86_64.zip
        unzip amd-catalyst-13.12-linux-x86.x86_64.zip && sh amd-catalyst-13.12-linux-x86.x86_64.run --force
        aticonfig --adapter=all --initial
        
1. APP SDK
> 下载 http://developer.amd.com/tools-and-sdks/heterogeneous-computing/amd-accelerated-parallel-processing-app-sdk/downloads/
        
配置
----
1. 自动登录

        insserv -r xdm
        #添加到/etc/rc.local
        su --login root -c '/bin/bash -l -c startx &> /dev/null' &
        
1. 关闭 DPMS
1. cgminer状态报告

        apt-get install php5-common libapache2-mod-php5 php5-cli
