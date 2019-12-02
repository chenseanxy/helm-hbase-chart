  
FROM chenseanxy/hadoop:3.2.1-nolib

ARG VERSION=

ADD hbase-${VERSION}-bin.tar.gz /opt

RUN cd /opt && ln -s ./hbase-${VERSION} hbase && \
    cp hbase/lib/client-facing-thirdparty/htrace-core-3.1.0-incubating.jar hbase/lib && \
    .

WORKDIR /opt/hbase
