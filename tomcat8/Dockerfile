# Centos based container with Java and Tomcat
FROM centos:latest

MAINTAINER hmesha

#Enable systemd support, which is disabled by default in centos image
ENV container docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
	systemd-tmpfiles-setup.service ] || rm -f $i; done); \
	rm -f /lib/systemd/system/multi-user.target.wants/*;\
	rm -f /etc/systemd/system/*.wants/*;\
	rm -f /lib/systemd/system/local-fs.target.wants/*; \
	rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
	rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
	rm -f /lib/systemd/system/basic.target.wants/*;\
	rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]

# Install prepare infrastructure
RUN yum -y update && \
	yum -y install tar && \
	yum -y install wget
# Install Java
RUN wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u144-b01/090f390dda5b47b9b721c7dfaa008135/jre-8u144-linux-x64.rpm && \
    echo "5f4b98cbe19862a969494ce878377298  jre-8u144-linux-x64.rpm" >> MD5SUM && \
    md5sum -c MD5SUM && \
    rpm -Uvh jre-8u144-linux-x64.rpm && \
    rm -f jre-8u144-linux-x64.rpm MD5SUM
RUN echo "`java -version`"
# Prepare JAVA environment 
ENV JAVA_HOME=/usr/java/jdk1.8.0_144
RUN echo "JAVA_HOME=$JAVA_HOME"



# Install Tomcat
ENV TOMCAT_GROUP tomcat
ENV TOMCAT_USER tomcat
ENV CATALINA_HOME /opt/tomcat
RUN groupadd $TOMCAT_GROUP
RUN useradd -M -s /bin/nologin -g tomcat -d /opt/tomcat $TOMCAT_USER
	
RUN wget http://mirror.dsrg.utoronto.ca/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz
RUN mkdir /opt/tomcat
RUN tar xvf apache-tomcat-8*tar.gz -C $CATALINA_HOME --strip-components=1
RUN cd /opt/tomcat
RUN chgrp -R tomcat $CATALINA_HOME
RUN chmod -R g+r $CATALINA_HOME/conf
RUN chmod g+x $CATALINA_HOME/conf
RUN chown -R $TOMCAT_USER $CATALINA_HOME/webapps/ $CATALINA_HOME/work/ $CATALINA_HOME/temp/ $CATALINA_HOME/logs/

# clean infrastucture
RUN yum -y remove wget

# Prepare Tomcat environment 
RUN echo "CATALINA_HOME=$CATALINA_HOME"

ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_HOME/scripts

#Enable admin console from remote host
COPY tomcat-users.xml $CATALINA_HOME/conf/tomcat-users.xml
COPY context.xml $CATALINA_HOME/webapps/host-manager/META-INF/context.xml
COPY context.xml $CATALINA_HOME/webapps/manager/META-INF/context.xml

#Configure tomcat as systemd service
COPY tomcat.service /etc/systemd/system
RUN systemctl enable tomcat

RUN mkdir -p /tmp/deployment
VOLUME ["/tmp/deployment"]

COPY start.sh .
RUN chmod +x start.sh
EXPOSE 8080

# To correctly start D-Bus, it should be the fist service - https://forums.docker.com/t/any-simple-and-safe-way-to-start-services-on-centos7-systemd/5695/8
CMD ["./start.sh"]