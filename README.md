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
1. 移除DisplayManager服务

        insserv -r xdm
        
1. 修改`/etc/rc.local`，添加自动配置X，关闭X电源管理，自动登录

        # 生成配置文件
        /usr/bin/aticonfig --adapter=all --initial
        # 关闭DPMS
        /bin/sed -i 's/^.*\bDPMS\b.*$/\tOption\t"DPMS"\t"false"/' /etc/X11/xorg.conf
        # 自动进入桌面
        /bin/su --login root -c '/bin/bash -l -c startx &> /dev/null' &
        # 自动启动挖矿
        /usr/local/bin/mine_start
        
1. 命令行自动自动
        
        #!/bin/sh
        set -e
        export DISPLAY=:0
        export GPU_MAX_ALLOC_PERCENT=100
        export GPU_USE_SYNC_OBJECTS=1
        #export XAUTHORITY=/.Xauthority
        screen -dmS miner bash -lc "echo 'waiting 15 seconds for X server starting...';sleep 15;/usr/local/bin/cgminer -c /etc/cgminer.conf;echo 'cgminer is quiting,please wait...';sleep 10;"

1. LXDE自动启动

        /etc/xdg/lxsession/LXDE/autostart

1. cgminer状态报告

        apt-get install php5-common libapache2-mod-php5 php5-cli

常见问题
--------
1. dri2 connection failed!
> cgminer参数设置 gpu-platform 0

1. 其它问题
> 待补充
