# dbcluster
<!-- 本机若无ansible，可通过脚本安装ansible -->
bash -x ansible/ansible_install.sh

<!-- 加载环境变量 -->
source ~/.bashrc

<!-- 默认读取主机配置生成参数，若主机非服务独享，即实际可用配置低于实际配置，可通过配置以下文件 -->
group_vars/all.yml：
    主机CPU数量：host_processor_cpus_default
    主机内存值：host_memtotal_mb_default

<!-- MySQL安装包默认使用2.17glibc的安装包,GreatDBRouter默认识别主机实际glibc进行选择，如需更改编辑以下文件： -->
group_vars/all.yml：
    MySQL安装包glibc版本：mysql_package_glibc
    GreatDBRouter安装包glibc版本:router_package_glibc

<!-- 默认客户端使用的是MySQL，如需变更客户端请更改以下文件[此变量和使用安装包相关]： -->
group_vars/all.yml：
    默认DB客户端：db_client

<!-- 默认 db 配置 innodb_buffer_pool_size 为主机的50%，如需变更请修改以下文件： -->
group_vars/all.yml：
    默认50%主机内存：innodb_buffer_pool_size

<!-- 部署时，使用默认安装包匹配，则配置包版本信息，安装包值保持为空； -->
[ inventory.yml 文件中 package_version info 部分 ]

<!-- 若使用指定安装包部署，则直接编辑安装包对应的值即可，版本信息无需关注； -->
[ inventory.yml 文件中 package_name info 部分 ] 

<!-- 操作系统包管理工具默认使用yum，若需要变更，更改以下文件 -->
roles/greatdbrouter/tasks/install_kazoo.yml

<!-- 部署默认为三节点，其他数量主机未做测试
安装包需位于此文件同级目录或对应role的file目录中 -->

<!-- jinja2配置文件路径 -->
MySQL OR GreatDB：roles/mysql/templates/my.cnf.j2
GreatDBRouter: roles/greatdbrouter/templates/dbscale.conf.j2

<!-- 操作系统glic版本相关配置 -->
Kylin glbc2.28
Centos/Redhat glibc2.17
package_glibc： glibc2.17 or glibc2.28
cpu_architectur: x86_64 or aarch64
group_vars/all.yml：
    DB安装包默认glic版本：db_package_glibc
    GreatDBRouter安装包默认glibc版本：router_package_glibc 【为空则工具根据系统进行选择，如果自定义则编辑该变量，仅支持2.17和2.28，其他版本请使用安装包进行部署】

<!-- 默认安装包名示例： -->
mysql-8.0.32-linux-glibc2.17-x86_64.tar.gz
    {{ db_client }}-{{ db_version }}-linux-glibc{{ db_package_glibc }}-{{ cpu_architectur }}.tar.gz
percona-xtrabackup-8.0.35-linux-glibc2.17-x86_64.tar.gz
    percona-xtrabackup-{{ percona_xtrabackup_version }}-linux-glibc{{ db_package_glibc }}-{{ cpu_architectur }}.tar.gz
jdk-8u111-linux-x86_64.tar.gz
    jdk-{{ jdk_version }}-linux-{{ cpu_architectur }}.tar.gz
zookeeper-3.8.3.tar.gz
    zookeeper-{{ zookeeper_version }}.tar.gz
GreatDBRouter-6.0.0.6224-LTS-2-85621274-Linux-glibc2.28-x86_64.tar.gz
    GreatDBRouter-{{ router_version }}-Linux-glibc{{ router_package_glibc  }}-{{ cpu_architectur }}.tar.gz

<!--
符合以上安装包默认规范时，只需要填写版本号即可，安装包信息无需关注；
若使用安装包直接匹配安装，则直接填写package_name info信息即可，package_version info信息无需关注，这部分信息只作为部署base路径命名 
-->

<!-- 配置示例 -->
<!-- 版本配置示例 -->
若使用如下安装包：
    mysql-8.0.36-linux-glibc2.17-x86_64.tar.gz
    percona-xtrabackup-8.0.35-linux-glibc2.17-x86_64.tar.gz
    jdk-8u111-linux-x86_64.tar.gz
    zookeeper-3.8.3.tar.gz
    GreatDBRouter-6.0.0.6192-LTS-1-102ea9ed-Linux-glibc2.17-x86_64.tar.gz
# package_version info
db_version: "8.0.36"
percona_xtrabackup_version: "8.0.35"
jdk_version: "8u111"
zookeeper_version: "3.8.3"
router_version: "6.0.0.6192-LTS-1-102ea9ed"
# package_name info
db_package_name_defined: ""
percona_xtrabackup_package_name_defined: ""
jdk_package_name_defined: ""
zookeeper_package_name_defined: ""
router_package_name_defined: ""
# 默认连接db客户端命令，mysql or greatdb
db_client: mysql

<!-- 安装包配置示例： -->
若使用如下安装包：
    mysql-8.0.36-linux-glibc2.17-x86_64.tar.gz
    percona-xtrabackup-8.0.35-26-34cf2908-linux-x86_64-glibc2.17.tar.gz
    jdk-8u111-linux-amd64.tar.gz
    zookeeper-3.8.3.tar.gz
    GreatDBRouter-6.0.0.6192-LTS-1-102ea9ed-Linux-glibc2.17-x86_64.tar.gz
# package_version info
jdk_version: "8u111"
db_version: "8.0.36"
router_version: "6.0.0.6192-LTS-1-102ea9ed"
zookeeper_version: "3.8.3"
percona_xtrabackup_version: "8.0.35"
# package_name info
db_package_name_defined: "mysql-8.0.36-linux-glibc2.17-x86_64.tar.gz"
jdk_package_name_defined: "jdk-8u111-linux-amd64.tar.gz"
router_package_name_defined: "GreatDBRouter-6.0.0.6192-LTS-1-102ea9ed-Linux-glibc2.17-x86_64.tar.gz"
percona_xtrabackup_package_name_defined: "percona-xtrabackup-8.0.35-26-34cf2908-linux-x86_64-glibc2.17.tar.gz"
zookeeper_package_name_defined: "zookeeper-3.8.3.tar.gz"
# 默认连接db客户端命令，mysql or greatdb
db_client: mysql
