  
FROM chenseanxy/hadoop:3.2.1-nolib

ARG VERSION=

ADD hbase-${VERSION}-bin.tar.gz /opt

RUN cd /opt && ln -s ./hbase-${VERSION} hbase

WORKDIR /opt/hbase

# REST API
EXPOSE 8080
# Thrift API
EXPOSE 9090

# Zookeeper port
EXPOSE 2181

# Master port
EXPOSE 16000
# Master info port
EXPOSE 16010

# Regionserver port
EXPOSE 16020
# Regionserver info port
EXPOSE 16030