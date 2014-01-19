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

1. 编译cgminer

        wget http://ck.kolivas.org/apps/cgminer/3.1/cgminer-3.1.1.tar.bz2
        tar jxf cgminer-3.1.1.tar.bz2
        cd cgminer-3.1.1/ADL_SDK && wget http://mein.intern3ts.com/ADL_SDK_5.0.zip && unzip -j ADL_SDK_5.0.zip 'include/adl_*.h'
        cd .. && ./configure --enable-scrypt && make && make install
        
配置
----
1. 自动登录，自动配置X，关闭X电源管理

        insserv -r xdm
        # 生成配置文件
        /usr/bin/aticonfig --adapter=all --initial
        # 关闭DPMS
        /bin/sed -i 's/^.*\bDPMS\b.*$/\tOption\t"DPMS"\t"false"/' /etc/X11/xorg.conf
        # 自动进入桌面
        /bin/su --login root -c '/bin/bash -l -c startx &> /dev/null' &
        
1. LXDE自动启动

        /etc/xdg/lxsession/LXDE/autostart

1. cgminer状态报告

        apt-get install php5-common libapache2-mod-php5 php5-cli
