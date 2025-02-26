# 작성자 김청운 작성일 20241109
# 이미지 작성 목적
# * 기존의 hive metastore는 hive cli를 사용할 수 없기 때문에
# * 메타스토어 관리를 위해서 hive cli가 포함된 이미지 구축
# -------------- version -----------------------
# v0.01 (2024.11.09): openjdk:24-slim-bookworm + hive4.0.1 image + mysql jdbc
# v0.02 (2024.11.10): v0.01 + hadoop3.3.6
# v0.03 (2024.11.10): v0.02 + 베이스 이미지 변경 ( openjdk:24-slim-bookworm -> openjdk:11 )
# v0.04 (2024.11.11): v0.03 + hadoop-aws-3.3.2.jar (에러 2) +  hive user 생성 & 파일 및 디렉토리 권한 설정
# v0.05 (2024.11.12): v0.04 + 추가 AWS JAVA SDK 설치 (java sdk, jackson-core,databind,annotations)

# openjdk:11 이미지를 베이스로 사용
FROM openjdk:11

# Hive,MySQL JDBC 드라이버 버전 선언
ARG MYSQL_JDBC_VERSION=8.4.0
ARG HIVE_VERSION=4.0.1
ARG HADOOP_VERSION=3.3.6

# Hive,MySQL JDBC 드라이버 URL 선언
ARG MYSQL_JDBC_URL=https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-j-${MYSQL_JDBC_VERSION}.tar.gz
ARG HIVE_URL=https://dlcdn.apache.org/hive/hive-${HIVE_VERSION}/apache-hive-${HIVE_VERSION}-bin.tar.gz
ARG HADOOP_URL=https://dlcdn.apache.org/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz

# App 구동 위해서 환경변수 설정
ENV HIVE_HOME=/usr/local/hive
ENV HADOOP_HOME=/usr/local/hadoop
ENV PATH=$HIVE_HOME/bin:$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# 필요한 App 설치 위해서 root계정 사용
USER root

# 'hive' 사용자 및 그룹 생성
RUN groupadd -r hive && useradd -r -g hive hive
# 필요한 App 설치 
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get install -y nano && \
    apt-get install -y wget

# Hadoop 설치
RUN mkdir -p ${HADOOP_HOME} && \
    wget -O ${HADOOP_HOME}/apache-hadoop-${HADOOP_VERSION}-bin.tar.gz ${HADOOP_URL} && \
    tar -xvzf ${HADOOP_HOME}/apache-hadoop-${HADOOP_VERSION}-bin.tar.gz -C ${HADOOP_HOME} --strip-components 1 && \
    rm -rf ${HADOOP_HOME}/apache-hadoop-${HADOOP_VERSION}-bin.tar.gz

# Hive, MySQL JDBC, hadoop-aws, java-aws-sdk 설치
RUN mkdir -p ${HIVE_HOME} && \
    wget -O ${HIVE_HOME}/apache-hive-${HIVE_VERSION}-bin.tar.gz ${HIVE_URL} && \
    tar -xvzf ${HIVE_HOME}/apache-hive-${HIVE_VERSION}-bin.tar.gz -C ${HIVE_HOME} --strip-components 1 && \
    rm ${HIVE_HOME}/apache-hive-${HIVE_VERSION}-bin.tar.gz && \
    cd ${HIVE_HOME}/lib && \
    wget ${MYSQL_JDBC_URL} && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/${HADOOP_VERSION}/hadoop-aws-${HADOOP_VERSION}.jar && \
    wget https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.10.3/jackson-core-2.10.3.jar && \
    wget https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.10.3/jackson-databind-2.10.3.jar && \
    wget https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.10.3/jackson-annotations-2.10.3.jar && \
    wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.12.367/aws-java-sdk-bundle-1.12.367.jar && \
    tar -xzvf mysql-connector-j-${MYSQL_JDBC_VERSION}.tar.gz --strip-components 1 && \
    rm -rf ${HIVE_HOME}/lib/${MYSQL_JDBC_VERSION}.tar.gz

# 파일 및 디렉토리 권한 설정
RUN chown -R hive:hive ${HIVE_HOME} && \
    chown -R hive:hive ${HADOOP_HOME}

WORKDIR /usr/local/hive
# 보안 위해서 계정 변경 
USER hive

