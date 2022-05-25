1、下载hadoop3.2.0	
2、设置主机IP,vi /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="1b8ce14f-64ab-4d03-80b5-63f04ad88eb8"
DEVICE="ens33"
ONBOOT="yes"
IPADDR=192.168.239.128
GATEWAY=192.168.239.1
DNS1=192.168.239.1
然后service network restart重启网络
3、设置hostname,vi /etc/hostname，改成bigdata01,systemctl restart systemd-hostnamed生效
4、关闭防火墙：systemctl disable firewalld
5、ssh免密登录：ssh-keygen -t rsa，一路回车，生成文件目录：ll ~/.ssh/
然后：cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
6、安装jdk
7、安装hadoop
a、hadoop的bin和sbin目录加入到环境变量
b、修改配置文件
1)hadoop-env.sh末尾加上
export JAVA_HOME=/home/cos/jjg/java/jdk1.8.0_202
export HADOOP_LOG_DIR=/data/hadoop_repo/logs/hadoop
2) vi core-site.xml等
core-site.xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://bigdata01:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/data/hadoop_repo</value>
   </property>
</configuration>

hdfs-site.xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>

mapred-site.xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>

yarn-site.xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
   <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>

workers
bigdata01
/////////////////////
start-dfs.sh
HDFS_DATANODE_USER=root
HDFS_DATANODE_SECURE_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root

stop-dfs.sh
HDFS_DATANODE_USER=root
HDFS_DATANODE_SECURE_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root

start-yarn.sh
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root

stop-yarn.sh
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
/////
8、格式化，hdfs namenode -format
///////////
9、启动,sbin/start-all.sh
端口，hdfs:9870,yarn8088