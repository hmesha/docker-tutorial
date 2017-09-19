Docker container: CentOS 7 + Java 8 + Tomcat 8


##Build Docker Image
'''git clone https://github.com/hmesha/docker-tutorial.git'''
'''cd docker-tutorial'''
'''docker build -t hmesha/java8-base java8-base/'''
'''docker build -t hmesha/tomcat8-base tomcat8-base/'''

##Run Docker Image

'''docker run --name tomcat8-base --cap-add SYS_ADMIN -p 8080:8080 -id -v /sys/fs/cgroup:/sys/fs/cgroup:ro hmesha/tomcat8-base'''

It exposes tomcat service running within container on port 8080 to host port 8080

Access Tomcat Admin console at: ''http://localhost:8080''

Admin Console Credentials: ''admin/admin''

##Deploy Web Application

On start, Docker container copies war files available at mount point '/usr/local/deployment' to $CATALINA_HOME/webapps directory of tomcat. To deploy web application(s) to this tomcat instance, create a host directory to contain all your war files, copy the war file(s) into it and and point it to the mounting to this docker mount point as shown below.

'cp web-application.war /host-dir/conatining/war-files'

'docker run --cap-add SYS_ADMIN -p 8080:8080 -id -v /sys/fs/cgroup:/sys/fs/cgroup:ro -v /host-dir/conatining/war-files:/usr/local/deployment:ro --name tomcat8-base hmesha/tomcat8-base'

##Start and Stop The Docker Container

In order to stop the above container, run the following command

'docker container stop tomcat8-base'

In order to start it again, run the following command

'docker container start tomcat8-base'

