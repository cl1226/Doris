# Doris
### 安装
1. 安装包
使用官方提供的编译好的包 ，地址：http://palo.baidu.com/docs/%E4%B8%8B%E8%BD%BD%E4%B8%93%E5%8C%BA/%E9%A2%84%E7%BC%96%E8%AF%91%E7%89%88%E6%9C%AC%E4%B8%8B%E8%BD%BD/
本次使用的是0.14.13.1版本
2. 角色安排
node01	fe	broker
node02  fe	broker
node03	be	broker
node03	be	broker
node04	be	broker
3. 修改fe/conf/fe.conf
http_port = 7031
rpc_port = 7032
query_port = 7033
edit_log_port = 7034
mysql_service_nio_enabled = true

priority_networks = node02/24
fe目录下创建目录doris_meta

4. 修改be/conf/be.conf
be_port = 7061
be_rpc_port = 7062
webserver_port = 7063
heartbeat_service_port = 7064
brpc_port = 7065
priority_networks = node03/24
be目录下创建目录storage

5. 修改apache_hdfs_broker/conf/apache_hdfs_broker.conf
broker_ipc_port=8000
将hdfs-site.xml 替换成hadoop集群的hdfs-site.xml

6. 启动，停止
fe/bin/start_fe.sh --daemon
fe/bin/stop_fe.sh

be/bin/start_be.sh --daemon
be/bin/stop_be.sh

apache_hdfs_broker/bin/start_broker --daemon
apahce_hdfs_broker/bin/stop_broker

7. 使用mysql客户端访问fe
mysql -uroot -hnode01 -P7033
增加fe：
alter system add frontend "node02:7034";
删除fe：
alter system drop frontend "node02:7034"
增加be：
alter system add backend "node03:7064";
alter system add backend "node04:7064";
alter system add backend "node05:7064";
删除be: 
alter system ddrop backend "node03:7064"; // 会删除数据，谨慎使用
增加broker:
alter system add broker broker_name "node02:8000","node03:8000","node04:8000","node05:8000"
