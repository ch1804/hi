# CC部署脚本

    使用时将cc目录移动到/data/lingban目录下

1. ***安装freeswitch***

    > cd freeswitch/op-scripts
    
    修改freeswitch的配置文件
    ```shell
    sh gen-op-scripts.sh
    #支持的参数，可按实际配置更改
    # -w path to fs workspace                                           freeswitch安装目录
    # -l path to log (default: /data/lingban/cc/freeswitch/log/)        freeswitch日志目录
    # -i local ip                                                       本机ip
    # -e external ip (optional, for fs behind NAT)                      公网ip
    # -h database host                                                  连接数据库的主机ip
    # -u database user                                                  连接数据库的账号
    # -s database password                                              连接数据库的密码
    # -r database port(default 3306)                                    连接数据库的端口
    # -d database name(default: cc)                                     连接数据库的库名
    # -n allow network segment                                          允许通行的网段
    # -m max-sessions(default: 4000)                                    最大连接数
    # -b sessions-per-second(default: 1000)                             每秒最大连接数
    # -j rtp-start-port(default: 16384)                                 rtp起始端口
    # -k rtp-end-port(default: 32768)                                   rtp终止端口
    # -p esl port (default: 8021)                                       给cm连接提供的端口
    # -a esl pass (default: ClueCon)                                    给cm连接提供的密码
    # -c apply-inbound-acl (default: lan)                               cm连接时的项目名
    # -x log maximum-rotate (default: 32)                               日志的最大保留数
    ```
    ```shell
    #启动fs
    sh fs.sh <start/stop/restart>
    ```
    ```shell
    #查看fs日志
    docker logs -f --tail=50 freeswitch
    ```
   
2. ***安装cm***

    > cd cm/op-scripts

    修改cm的配置文件
    ```shell
    sh gen-op-scripts.sh
    #支持的参数，可按实际配置更改
    # -w path to fs workspace"                                         cm安装目录
    # -l path to log (default: cm/log)"                                cm日志位置
    # -i local ip"                                                     本机ip
    # -a recordings dir"                                               录音存放位置
    # -h database host"                                                连接数据库的主机ip
    # -r database port(default 3306)"                                  连接数据库的端口
    # -u database user"                                                连接数据库的账号
    # -s database pass"                                                连接数据库的密码
    # -d database name(default cc)"                                    连接数据库的库名
    # -m freeswicth host"	                                           freeswitch所在主机ip
    # -p freeswicth port(default: 8021)"                               freeswitch提供的连接端口
    # -c freeswicth pass (default: ClueCon)"                           freeswitch提供的密码
    # -e log size(default: 200)"                                       日志的大小
    # -f log num(default: 10)"                                         日志的个数
    # -g error log size(default: 100)"                                 错误日志的大小
    # -j error log num(default: 20)"                                   错误日志的个数
    ```
    ```shell
    #启动cm
    sh cm.sh <start/stop/restart>
    ```
    ```shell
    #查看cm日志
    docker logs -f --tail=50 cm
    ```
   
3. ***安装ccs,ccadmin***

    > cd ccs_ccadmin
    ```shell
    #按实际配置修改cc.conf
    MYSQL_HOST=1.1.1.1
    MYSQL_PORT=1111
    MYSQL_USER=11
    MYSQL_PWD=11
    DB_NAME=11
    REDIS_HOST=2.2.2.2
    REDIS_PORT=22
    REDIS_PWD=22
    #cm address
    SS_HOST=3.3.3.3
    #log size  M
    FILESIZE=44
    #log number
    MAXHISTORY=4
    #allow network segment
    INTERFACES=192.168.145.0/24
    LOCAL_IP=192.168.145.144
    DISCOVERY_HOST=6.6.6.6
    ```
      
4. ***生成env,log_conf配置文件以及docker-compose.yml***
    ```shell
    sh install-env.sh -d /data/lingban/cc
    ```
5. ***启动ccs,ccadmin***
    ```shell
    cd /data/lingban/cc
    docker-compose up -d
    ```
