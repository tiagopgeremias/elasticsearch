# elasticsearch

Installation Elasticsearch


## Disable Firewall

```sh
systemctl stop firewalld
systemctl disable firewalld
```

## Disable SELinux

```sh
sed -i 's/^SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
setenforce 0
reboot 
```

## Update System

```sh
yum -y update
```

## Install JAVA

```sh
yum -y install java-1.8.0-openjdk  java-1.8.0-openjdk-devel
```

## Set Java Home

```sh
cat <<EOF | sudo tee /etc/profile.d/java8.sh
export JAVA_HOME=/usr/lib/jvm/jre-openjdk
export PATH=\$PATH:\$JAVA_HOME/bin
export CLASSPATH=.:\$JAVA_HOME/jre/lib:\$JAVA_HOME/lib:\$JAVA_HOME/lib/tools.jar
EOF
```

Load JAVA environments variables

```sh
source /etc/profile.d/java8.sh
```

## Add Elasticsearch repository

```sh
cat <<EOF | sudo tee /etc/yum.repos.d/elasticsearch.repo
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/oss-7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
EOF
```

Clean yum cache

```sh
yum clean all
yum makecache
```

## Install Elasticsearch

```sh
yum -y install elasticsearch-oss
```

## Configure JVM Memory

If is necessary change JVM of configuration, edit of Xms and Xmx params, any values must be equals

```sh
vi /etc/elasticsearch/jvm.options
```

## Configure cluster properties.

Change properties values to your cluster values

```sh
sed -i "s/^#cluster.name:.*/cluster.name: my-elastic-cluster/g" /etc/elasticsearch/elasticsearch.yml
sed -i "s/^#node.name:.*/node.name: node-001/g" /etc/elasticsearch/elasticsearch.yml
sed -i "s/^#network.host:.*/network.host: 0.0.0.0/g" /etc/elasticsearch/elasticsearch.yml
sed -i "s/^#http.port:.*/http.port: 9200/g" /etc/elasticsearch/elasticsearch.yml
sed -i "s/^#http.port:.*/http.port: 9200/g" /etc/elasticsearch/elasticsearch.yml
sed -i "s/^#discovery.seed_hosts:.*/discovery.seed_hosts: [\"0.0.0.0\"]/g" /etc/elasticsearch/elasticsearch.yml
#discovery.seed_hosts: .*

```

## Enable and Start Elasticsearch

```sh
systemctl enable --now elasticsearch
```

## Test Elasticsearch endpoint

```sh
curl http://127.0.0.1:9200
```