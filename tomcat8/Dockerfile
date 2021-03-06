# Centos based container with Java and Tomcat
FROM hmesha/java8

MAINTAINER hmesha

# Install prepare infrastructure
RUN yum -y update && \
	yum -y install tar && \
	yum -y install wget

# Install Tomcat
ENV TOMCAT_GROUP tomcat
ENV TOMCAT_USER tomcat
ENV CATALINA_HOME /opt/tomcat
RUN groupadd $TOMCAT_GROUP
RUN useradd -M -s /bin/nologin -g $TOMCAT_GROUP -d $CATALINA_HOME $TOMCAT_USER
	
RUN wget http://mirror.dsrg.utoronto.ca/apache/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz
RUN mkdir $CATALINA_HOME
RUN tar xvf apache-tomcat-8*tar.gz -C $CATALINA_HOME --strip-components=1
RUN cd $CATALINA_HOME
RUN chown -R $TOMCAT_USER:$TOMCAT_GROUP $CATALINA_HOME
RUN chmod -R g+r $CATALINA_HOME/conf
RUN chmod g+x $CATALINA_HOME/conf

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

RUN mkdir -p /tmp/tomcat/deployments
VOLUME ["/tmp/tomcat/deployments"]

EXPOSE 8080

WORKDIR $CATALINA_HOME/bin
COPY start_deployment.sh .
RUN chmod +x start_deployment.sh

# https://forums.docker.com/t/any-simple-and-safe-way-to-start-services-on-centos7-systemd/5695/8
CMD ["./start_deployment.sh"]