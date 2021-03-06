# 基礎安裝部份請參考 singleNode 安裝流程

## 請用 VB 或是 VMware 啟動4台主機（包含 Host 本機）

## 設定主機資訊（每一台）
```
sudo vi /etc/hosts
```
以下為我的電腦設定，請依個人需求更改
```
192.168.1.150 nn
192.168.1.151 dn01
192.168.1.152 dn02
192.168.1.153 dn03
```

## 發送金鑰

## 修改 Hadoop 組態設定檔

### core-site.xml(nn與dn相同)
```
<configuration>
<property>
  <name>fs.default.name</name>
  <value>hdfs://nn:9000</value>
</property>
</configuration>
```

### hdfs-site.xml(nn)
```
<configuration>
<property>
 <name>dfs.replication</name>
 <value>3</value>
</property>
<property>
  <name>dfs.namenode.name.dir</name>
  <value>file:///home/hadoop/hadoop_data/hdfs/namenode</value>
</property>
</configuration>
```

### hdfs-site.xml(dn)
```
<configuration>
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
<property>
  <name>dfs.datanode.data.dir</name>
  <value>file:///home/hadoop/hadoop_data/hdfs/datanode</value>
</property>
</configuration>
```

### mapred-site.xml(nn)
```
<configuration>
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
<property>
  <name>mapreduce.jobhistory.address</name>
  <value>nn:10020</value>
</property>
</configuration>
```

### mapred-site.xml(dn)
```
<configuration>
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
<property>
  <name>mapreduce.jobtracker.address</name>
  <value>nn:54311</value>
</property>
</configuration>
```

### yarn-site.xml(nn與dn相同)
```
<configuration>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
  <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<property>
  <name>yarn.resourcemanager.resource-tracker.address</name>
  <value>nn:8025</value>
</property>
<property>
  <name>yarn.resourcemanager.scheduler.address</name>
  <value>nn:8030</value>
</property>
<property>
  <name>yarn.resourcemanager.address</name>
  <value>nn:8050</value>
</property>
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>nn</value>
</property>
<property>
　<name>yarn.nodemanager.vmem-check-enabled</name>
　<value>false</value>
</property>
<property>
   <name>yarn.nodemanager.pmem-check-enabled</name>
   <value>false</value>
</property>
</configuration>
```

## 建立HDFS目錄
nn連線至dn01、dn02、dn03建立DataNode HDFS目錄
```
ssh dn01,dn02,dn03
rm -rf /home/hadoop/hadoop_data/hdfs
mkdir -p /home/hadoop/hadoop_data/hdfs/datanode
exit
```

建立NameNode HDFS 目錄
```
rm -rf /home/hadoop/hadoop_data/hdfs
mkdir -p /home/hadoop/hadoop_data/hdfs/namenode
```

NameNode格式化 HDFS 目錄
```
hadoop namenode -format
```

啟動Hadoop Cluster
```
start-dfs.sh
start-yarn.sh
```
## 檢視hadoop是否順利啟動
```
jps
```
## 可連線至:http://192.168.1.150:50070查看是否順利啟動各Datanode


關閉Hadoop Cluster
```
stop-dfs.sh
stop-yarn.sh
```
