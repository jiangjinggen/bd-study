1������hadoop3.2.0	
2����������IP,vi /etc/sysconfig/network-scripts/ifcfg-ens33
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
Ȼ��service network restart��������
3������hostname,vi /etc/hostname���ĳ�bigdata01,systemctl restart systemd-hostnamed��Ч
4���رշ���ǽ��systemctl disable firewalld
5��ssh���ܵ�¼��ssh-keygen -t rsa��һ·�س��������ļ�Ŀ¼��ll ~/.ssh/
Ȼ��cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
6����װjdk
7����װhadoop
a��hadoop��bin��sbinĿ¼���뵽��������
b���޸������ļ�
1)hadoop-env.shĩβ����
export JAVA_HOME=/home/cos/jjg/java/jdk1.8.0_202
export HADOOP_LOG_DIR=/data/hadoop_repo/logs/hadoop
2) vi core-site.xml��
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
8����ʽ����hdfs namenode -format
///////////
9������,sbin/start-all.sh
�˿ڣ�hdfs:9870,yarn8088