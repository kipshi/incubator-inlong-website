---
title: 安装部署
sidebar_position: 2
---

```
cd inlong-agent
```

## 配置

线上运行需要从inlong-manager拉取配置，配置conf/agent.properties如下：
```ini
agent.local.ip=写入本机ip
agent.manager.vip.http.host=manager host
agent.manager.vip.http.port=manager port
# audit proxy 地址
audit.proxys=127.0.0.1:10081
```
## 依赖
- 如果后端连接 MySQL 数据库，请下载 [mysql-connector-java-8.0.26.jar](https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.26/mysql-connector-java-8.0.26.jar), 并将其放入 `lib/` 目录。
- 如果后端连接 PostgreSQL 数据库，不需要引入额外依赖。

## 启动

解压后如下命令运行
```bash
sh agent.sh start
```

## 实时添加job配置

```bash
curl --location --request POST 'http://localhost:8008/config/job' \
--header 'Content-Type: application/json' \
--data '{
"job": {
"dir": {
"path": "",
"pattern": "/data/inlong-agent/test.log"
},
"trigger": "org.apache.inlong.agent.plugin.trigger.DirectoryTrigger",
"id": 1,
"thread": {
"running": {
"core": "4"
}
},
"name": "fileAgentTest",
"source": "org.apache.inlong.agent.plugin.sources.TextFileSource",
"sink": "org.apache.inlong.agent.plugin.sinks.ProxySink",
"channel": "org.apache.inlong.agent.plugin.channel.MemoryChannel"
},
"proxy": {
"inlongGroupId": "groupId10",
"inlongStreamId": "streamId10"
},
"op": "add"
}'
```

其中各个参数含义为：
- job.dir.pattern: 配置读取的文件路径，可包含正则表达式
- job.trigger: 触发器名称，默认为DirectoryTrigger，功能为监听文件夹下的文件产生事件，任务运行时已有的文件不会读取
- job.source: 使用的数据源类型，默认为TextFileSource，读取文本文件
- job.sink：使用的写入器类型，默认为ProxySink，发送消息到dataproxy中
- proxy.groupId: 写入proxy时使用的groupId，groupId是指manager界面中，数据接入中业务信息的业务ID，此处不是创建的tube topic名称
- proxy.streamId: 写入proxy时使用的streamId，streamId是指manager界面中，数据接入中数据流的数据流ID