Docker container: CentOS 7 + Java 8 + Tomcat 8


##Build Docker Image
'''git clone https://github.com/hmesha/docker-tutorial.git'''
'''cd docker-tutorial'''
'''docker build -t hmesha/java8 java8/'''
'''docker build -t hmesha/tomcat8 tomcat8/'''

##Run Docker Image

'''docker run --name tomcat8 --cap-add SYS_ADMIN -p 8080:8080 -id -v /sys/fs/cgroup:/sys/fs/cgroup:ro hmesha/tomcat8'''

It exposes tomcat service running within container on port 8080 to host port 8080

Access Tomcat Admin console at: ''http://localhost:8080''

Admin Console Credentials: ''admin/admin''

##Deploy Web Application

On start, Docker container copies war files available at mount point (e.g. '/tmp/tomcat/deployment') to $CATALINA_HOME/webapps directory of tomcat. To deploy web application(s) to this tomcat instance, create a host directory to contain all your war files, copy the war file(s) into it and include mapping in the run command to this docker mount point as shown below.

'cp web-application.war /tmp/tomcat/deployments'

'docker run --cap-add SYS_ADMIN -p 8080:8080 -id -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v /tmp/tomcat/deployments:/tmp/tomcat/deployments:ro --name tomcat8 hmesha/tomcat8'

##Start and Stop The Docker Container

In order to stop the above container, run the following command

'docker stop tomcat8'

In order to start it again, run the following command

'docker start tomcat8'

