```bash

# 注意运行时，不要从vagrant连接到windows文件夹，否则会功能异常

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6381.conf

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6382.conf

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6383.conf

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6391.conf

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6392.conf

/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6393.conf

#创建集群
redis-cli --cluster create 127.0.0.1:6381 127.0.0.1:6382 127.0.0.1:6383 127.0.0.1:6391 127.0.0.1:6392 127.0.0.1:6393 --cluster-replicas 1

#查看集群信息
/data/redis-cluster/bin/redis-cli -p 6381 cluster info

#查看节点信息
/data/redis-cluster/bin/redis-cli -p 6381 cluster nodes

#客户端连接
redis-cli -c -p 6381
>set hello world
>get hello
>cluster nodes
>cluster info

#故障模拟
redis-cli -p 6382 debug segfault

#重新分配slot
redis-cli --cluster reshard 127.0.0.1:6381

redis-cli --cluster reshard <host>:<port> --cluster-from <node-id> --cluster-to <node-id> --cluster-slots <number of slots> --cluster-yes


#检查集群
redis-cli --cluster check 127.0.0.1:6381


#添加节点
/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6384.conf
/data/redis-cluster/bin/redis-server /data/redis-cluster/conf/redis-6394.conf

redis-cli --cluster add-node 127.0.0.1:6384 127.0.0.1:6381
or
redis-cli --cluster add-node 127.0.0.1:6384 127.0.0.1:6381 --cluster-slave
or
redis-cli --cluster add-node 127.0.0.1:6384 127.0.0.1:6381 --cluster-slave --cluster-master-id xxxx

#删除节点
redis-cli --cluster del-node 127.0.0.1:6381 `<node-id>`

```