# mammuthus-yarn-docker-scheduler
基于Yarn的容器调度引擎(container scheduler based on yarn)

## 关联项目

[https://github.com/allwefantasy/mammuthus-yarn-client](https://github.com/allwefantasy/mammuthus-yarn-client)   使得构建Yarn程序更加容易
[https://github.com/allwefantasy/mammuthus-dynamic-deploy](https://github.com/allwefantasy/mammuthus-dynamic-deploy) 基于mammuthus-yarn-client，可以调度Java/Docker 容器的系统
[https://github.com/allwefantasy/DCS](https://github.com/allwefantasy/DCS) 一个示例Java Web程序，可以被mammuthus-dynamic-deploy 调度


## 运行这个项目

上面所有的项目都基于 [https://github.com/allwefantasy/ServiceFramework](https://github.com/allwefantasy/ServiceFramework) 构建。你需要能够编译和打包该项目。正常情况下这个项目只要

```
mvn -DskipTests  install 
```
到你本地。或者

```
mvn -DskipTests  deploy 
```
到你的私仓。

ServiceFramework 项目主页会告诉你依赖的一个子项目。按照相同的方式打包到本地或者私仓即可。


要求：

1. Yarn 目前只测试过 2.6版本
2. ServiceFramework 为最新master版本。1.3.0-SNAPSHOT
3. JDK 1.7

## 如何构建 mammuthus-yarn-client/mammuthus-dynamic-deploy

你需要将Hadoop相关配置文件放到 mammuthus-yarn-client 的resources目录(没有的话自己创建该目录)之后使用

```
mvn package
```
获得一个完整的包

对于mammuthus-dynamic-deploy 项目，也是通过

```
mvn package
```
获得一个完整的包

## 如何构建示例程序DCP

1. git clone https://github.com/allwefantasy/DCS .
2. ./app.sh launch_dcs.sh compile
3. tar czvf DCS@1.0.tar.gz DCS

`@` 符号比较特殊，是为了分割应用名称和版本的

你需要将该项目放到一个可以提供http下载的地址。我在示例中，比如 `http://appstore:9003/DCS@1.0.tar.gz`

## 启动测试


```
java -cp /Users/allwefantasy/CSDNWorkSpace/mammuthus-yarn-client/target/mammuthus-yarn-client-1.0-SNAPSHOT-SHADED.jar mammuthus.yarn.Client \
--jar /Users/allwefantasy/CSDNWorkSpace/mammuthus-dynamic-deploy/target/mammuthus-dynamic-deploy-1.0-SNAPSHOT-jar-with-dependencies.jar \ 
--driver-memory 256m \
--num-executors 2 \
--executor-memory 512m  \
--class mammuthus.deploy.dynamic.DynamicDeployMaster \
--slave-class mammuthus.deploy.dynamic.DynamicDeploySlave \
--arg "http://appstore/DCS@1.0.tar.gz" \
--arg "/tmp/DCS-dev@1.0.tar.gz" \
--arg "./app.sh launch_dcs.sh start" \
--arg "http://appstore" 
```

说明下：

1. 第一个-cp 后的包为mammuthus-yarn-client的package 包
2. --jar 为mmauthus-dynamic-deploy
3. 第一个--arg 是 DCS的下载地址
4. 第二个--arg在各个节点下载时，临时目录
5. 第三个--arg是DCS的启动指令
6. 第四个--arg 可暂时为空，或者随便填写一个。该地址原本是为了和分布式管理系统通讯的。





