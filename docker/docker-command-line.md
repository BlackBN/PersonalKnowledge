# docker 命令

```bash
docker ps -s #查看哪些容器正在运行
docker exec -it a93f15468550 bash #进入容器
docker build -t ceres-java . 
docker tag 377e8ea31146 cargo.dev.caicloud.xyz/test/ceres-java:v1.0.44 #打tag
docker login -u develop -p Pwd123456 cargo.dev.caicloud.xyz #登录远程镜像仓库
docker push cargo.dev.caicloud.xyz/test/ceres-java:v1.0.44 #push镜像
docker cp  nginx:/etc/nginx/nginx.conf  /etc/nginx/  #拷贝容器中的配置文件 /etc/nginx/nginx.conf 到宿主机上
docker cp  /nginx/nginx.conf nginx:/etc/nginx/ #拷贝宿主机上的 /nginx/nginx.conf 文件到容器内
```

```bash
docker run -d -it --name prometheus --restart=always --link node-exporter -p 9090:9090 -v ~/Prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

docker run -it -d --restart=always --name mongodb -v /Users/caicloud/MongoDB/data:/data/db  -p 27017:27017 mongo

docker run --restart=always -d --name es-log -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.2

docker run -it -d --restart=always --name logstash -p 5044:5044 -p 9600:9600  logstash:6.8.2

docker run -it -d --restart=always --name logstash -p 5044:5044 -p 9600:9600 -v ~/Logstash/Config/:/usr/share/logstash/config/ logstash:6.8.2


```

```bash
docker run --restart=always --net=host --name consul-server -v /app/consul/data:/consul/data -v /app/consul/conf:/consul/config -d docker.io/consul consul agent -server -bind=192.168.29.128 -client 0.0.0.0 -ui -bootstrap-expect=2 -data-dir /consul/data -config-dir /consul/config

docker run --restart=always --net=host --name consul -v /app/consul/data:/consul/data -v /app/consul/conf:/consul/config -d docker.io/consul consul agent -server -bind=192.168.29.129 -client 0.0.0.0 -ui -bootstrap-expect=2 -data-dir /consul/data -config-dir /consul/config -join 192.168.29.128

docker run -d --restart=always --name consul-server -p 8500:8500 -v /Users/bn/Documents/Technology/docker/consul/data:/consul/data -v /Users/bn/Documents/Technology/docker/consul/config:/consul/config docker.io/consul agent -server -bootstrap -ui -node=1 -client='0.0.0.0'

```

```bash
启动 node-exporter
docker run -d --restart=always --name node-exporter -p 9100:9100 -v "/Users/bn/Documents/Technology/docker/node-exporter/proc:/host/proc:ro" -v "/Users/bn/Documents/Technology/docker/node-exporter/sys:/host/sys:ro" -v "/Users/bn/Documents/Technology/docker/node-exporter/:/rootfs:ro" prom/node-exporter

启动 prometheus
docker run -d --restart=always --link node-exporter --name prometheus -p 9090:9090 -v /Users/bn/Documents/Technology/docker/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus

启动 grafana
docker run -d --restart=always --link prometheus --link mysql --name=grafana -p 3000:3000 -v /Users/bn/Documents/Technology/docker/grafana/grafana-storage:/var/lib/grafana grafana/grafana
```

```bash
#原文：https://www.jianshu.com/p/4807cfb5861a

docker run -d --restart=always -p 0.0.0.0:9200:9200 -p 9300:9300 --name elasticsearch -e "discovery.type=single-node" elasticsearch:7.5.2

docker run -d --restart=always -p 5601:5601 --link elasticsearch -e ELASTICSEARCH_URL=http://elasticsearch:9200 kibana:7.5.2

docker run -d --restart=always --name zookeeper -p 2181:2181 wurstmeister/zookeeper

docker run -d -p 9092:9092 --name kafka --link zookeeper --add-host kafka:127.0.0.1 -e \
KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 -e \
KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092 -e \
KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka


docker run -d -it --restart=always --name filebeat --link kafka -v /Users/bn/Documents/Technology/docker/elk/filebeat.yml:/usr/share/filebeat/filebeat.yml -v  /Users/bn/Documents/Technology/docker/elk/logs/:/opt/log_test/ docker.elastic.co/beats/filebeat:6.8.2

docker run -d -it --restart=always -p 5044:5044 --name logstash --link elasticsearch --link kafka -v /Users/bn/Documents/Technology/docker/elk/logstash.conf:/usr/share/logstash/pipeline/logstash.conf docker.elastic.co/logstash/logstash:7.5.2

```

```bash
启动apisix

# 切换到安装目录，如/opt
cd /opt
#将 Apache APISIX 的 Docker 镜像下载到本地
git clone https://github.com/apache/apisix-docker.git

#赋权限(不执行，会报：ETCD容器启动时报错：cannot access data directory: mkdir /bitnami/etcd/data: permission denied，表示etcd启动时未没有创建db的权限)
chmod -R 777 /opt/apisix-docker/example/

# 将当前的目录切换到 apisix-docker/example 路径下
cd apisix-docker/example
# 运行 docker-compose 命令，安装 Apache APISIX
docker-compose -p docker-apisix up -d


#拉取镜像
docker-compose pull
#创建并启动镜像
docker-compose -p docker-apisix up -d
#停止并删除镜像
docker-compose -p docker-apisix down
#启动服务
docker-compose -p docker-apisix start
#停止服务
docker-compose -p docker-apisix stop

```

```bash
创建 mysql :
docker run -it --name=mysql-test --restart=always -e MYSQL_DATABASE=test -e MYSQL_USER=test -e MYSQL_PASSWORD=test -e MYSQL_ROOT_PASSWORD=root -v /Users/bn/Documents/Technology/docker/mysql-test:/var/lib/mysql -p 3307:3306 -d mysql:5.7
```

```bash
创建 nacos :

docker run -d \
--link default_mysql:default_mysql \
-e MODE=standalone \
-e SPRING_DATASOURCE_PLATFORM=mysql \
-e MYSQL_SERVICE_HOST=default_mysql \
-e MYSQL_SERVICE_PORT=3306 \
-e MYSQL_SERVICE_USER=test \
-e MYSQL_SERVICE_PASSWORD=test \
-e MYSQL_SERVICE_DB_NAME=test \
-p 8848:8848 \
--restart=always \
--name mynacos \
nacos/nacos-server:v2.0.4

需要本地运行 default_mysql 容器
创建 test db 
并创建相应表 https://github.com/alibaba/nacos/blob/master/distribution/conf/nacos-mysql.sql

```

```bash
创建 nginx :
docker run -d -p 80:80 --restart=always --name nginx-web  -v /Users/bn/Documents/Technology/docker/nginx/nginx.conf:/etc/nginx/nginx.conf nginx:1.19.0-alpine
```
