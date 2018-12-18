# Copyright 2014 Netflix, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM netflixoss/java:7
MAINTAINER Netflix Open Source Development <talent@netflix.com>
ARG ZOOKEEPER_VERSION=3.4.13
USER root

RUN apt-get update &&\
  apt-get -y install ca-certificates &&\
  wget -q http://archive.apache.org/dist/zookeeper/zookeeper-$ZOOKEEPER_VERSION/zookeeper-$ZOOKEEPER_VERSION.tar.gz.md5 &&\
  wget -q http://archive.apache.org/dist/zookeeper/zookeeper-$ZOOKEEPER_VERSION/zookeeper-$ZOOKEEPER_VERSION.tar.gz &&\
  md5sum -c zookeeper-$ZOOKEEPER_VERSION.tar.gz.md5 &&\
  tar xzf zookeeper-$ZOOKEEPER_VERSION.tar.gz &&\
  rm /zookeeper-$ZOOKEEPER_VERSION.tar.gz* &&\
  mv /zookeeper-$ZOOKEEPER_VERSION /zookeeper
  
RUN apt-get -y install maven &&\
  mkdir /exhibitor &&\
  cd /exhibitor

ADD pom.xml /exhibitor/pom.xml
  
RUN cd /exhibitor &&\
  mvn clean package

RUN cd /exhibitor &&\
  mv target/exhibitor-1.6.0.jar ./ &&\
  rm -rf /exhibitor/target &&\
  rm /exhibitor/pom.xml
  
ADD exhibitor.properties /exhibitor/exhibitor.properties

EXPOSE 2181 2888 3888 8080

WORKDIR /exhibitor

ENTRYPOINT ["java", "-jar", "exhibitor-1.6.0-shaded.jar", "-c", "file"]
