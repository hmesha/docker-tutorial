FROM hmesha/centos7

RUN yum -y install wget && \
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jre-8u144-linux-x64.rpm && \
    echo "5f4b98cbe19862a969494ce878377298  jre-8u144-linux-x64.rpm" >> MD5SUM && \
    md5sum -c MD5SUM && \
    rpm -Uvh jre-8u144-linux-x64.rpm && \
    yum -y remove wget && \
    rm -f jre-8u144-linux-x64.rpm MD5SUM
RUN echo "`java -version`"

ENV JAVA_HOME=/usr/java/jdk1.8.0_144

RUN echo "JAVA_HOME=$JAVA_HOME"
