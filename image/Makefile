.PHONY: build

# Current apache hbase releases: http://hbase.apache.org/releases.html

HADOOP_30_VERSION = 3.1.2
HBASE_VERSION = 1.4.10

DOCKER = docker
DOCKER_REPO = chenseanxy/hbase

all: build

### Build hbase versioned images with native libs.
build:
	$(DOCKER) build -t hbase:$(HBASE_VERSION)-hadoop$(HADOOP_30_VERSION) \
	--build-arg HADOOP_VERSION=$(HADOOP_30_VERSION) \
	--build-arg VERSION=$(HBASE_VERSION) \
	.

tag: 
	$(DOCKER) tag \
	hbase:$(HBASE_VERSION)-hadoop$(HADOOP_30_VERSION) \
	$(DOCKER_REPO):$(HBASE_VERSION)-hadoop$(HADOOP_30_VERSION)

push: 
	$(DOCKER) push $(DOCKER_REPO):$(HBASE_VERSION)-hadoop$(HADOOP_30_VERSION)

### Fetch binary distribution
hbase-%-bin.tar.gz:
	curl -LO http://archive.apache.org/dist/hbase/$(HBASE_VERSION)/hbase-$(HBASE_VERSION)-bin.tar.gz

# Keep intermediate downloads.
.PRECIOUS: hbase-%-bin.tar.gz
