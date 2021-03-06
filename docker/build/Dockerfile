#-----------------------------------------------------------------------
# iroha-rel - IROHA container for Release
#
# build : docker build -t hyperledger/iroha .
#
# Copyright (c) 2016 Soramitsu,Co.,Ltd.
# All Rights Reserved.
#-----------------------------------------------------------------------

FROM ubuntu

MAINTAINER Takeshi Yonezu <yonezu@soramitsu.co.jp> / Bogdan Vaneev <bogdan@soramitsu.co.jp>

LABEL BUILD="docker build -t hyperledger/iroha ."
LABEL RUN="docker run -d --name iroha hyperledger/iroha"

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
# IROHA_HOME here should be the same as IROHA_RELEASE in iroha-dev
ENV IROHA_HOME=/usr/local/iroha

# this copies iroha and unpacks its binaries
ADD ./iroha.tar /
ADD ./scripts /

RUN groupadd -g 168 iroha && \
    useradd -u 168 -c "IROHA Administrator" -s /bin/bash -m -d ${IROHA_HOME} -p $(perl -e "print(crypt('passw0rd', 'sa'));") -g iroha iroha && \
    usermod -G adm,sudo iroha && \
    echo "export IROHA_HOME=${IROHA_HOME}" >> ${IROHA_HOME}/.bashrc && \
    echo "export JAVA_HOME=${JAVA_HOME}" >> ${IROHA_HOME}/.bashrc; \

    apt update && apt -y upgrade && apt -y install default-jdk netcat libsnappy-dev; \

    echo /usr/local/lib >>/etc/ld.so.conf && \
    echo ${IROHA_HOME}/lib >>/etc/ld.so.conf && \
    ldconfig

CMD ["/run.sh"]
