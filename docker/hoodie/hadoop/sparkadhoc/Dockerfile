
#  Licensed to the Apache Software Foundation (ASF) under one
#  or more contributor license agreements.  See the NOTICE file
#  distributed with this work for additional information
#  regarding copyright ownership.  The ASF licenses this file
#  to you under the Apache License, Version 2.0 (the
#  "License"); you may not use this file except in compliance
#  with the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
# limitations under the License.

ARG HADOOP_VERSION=2.8.4 
ARG HIVE_VERSION=2.3.3
ARG SPARK_VERSION=2.4.4
FROM apachehudi/hudi-hadoop_${HADOOP_VERSION}-hive_${HIVE_VERSION}-sparkbase_${SPARK_VERSION}

ARG PRESTO_VERSION=0.240
COPY adhoc.sh /opt/spark
 
ENV SPARK_WORKER_WEBUI_PORT 8081
ENV SPARK_WORKER_LOG /spark/logs
ENV SPARK_MASTER "spark://spark-master:7077"
ENV PRESTO_VERSION ${PRESTO_VERSION}

RUN set -x \
    ## presto-client
    && DEBIAN_FRONTEND=noninteractive apt-get -yq update \
    && apt-get -yq install vim \
    && wget -q -O /usr/local/bin/presto https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/${PRESTO_VERSION}/presto-cli-${PRESTO_VERSION}-executable.jar \
    && wget -q -O /opt/presto-spark-launcher-${PRESTO_VERSION}.jar https://repo1.maven.org/maven2/com/facebook/presto/presto-spark-launcher/${PRESTO_VERSION}/presto-spark-launcher-${PRESTO_VERSION}.jar \
    && wget -q -O /opt/presto-spark-package-${PRESTO_VERSION}.tar.gz https://repo1.maven.org/maven2/com/facebook/presto/presto-spark-package/${PRESTO_VERSION}/presto-spark-package-${PRESTO_VERSION}.tar.gz \
    && chmod +x /usr/local/bin/presto \
    && echo query.hash-partition-count=10 > /opt/config.properties \
    && echo redistribute-writes=false >> /opt/config.properties \
    && echo "select 1" > 1.sql \
    && mkdir -p /opt/presto240/etc/catalog

COPY hive.properties /opt/presto240/etc/catalog   
CMD ["/bin/bash", "/opt/spark/adhoc.sh"]
