Docker container: CentOS 7 + Java 8 + Wildfly 10.1.0.Final

##Build the image

'''docker build -t hmesha/wildfly10 wildfly10/'''

## Exposing WildFly Admin Console in Docker image

. Run container and bind application and management ports to all network interfaces as shown:
'''
docker run --name wildfly10 -p 8080:8080 -p 9990:9990 -d hmesha/wildfly10 /opt/jboss/wildfly/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0
'''
+
. The `-P` flag map any network ports inside the image it to a random high port from the range 49153 to 65535 on Docker host. Exact port can be verified by giving `docker ps` command as shown:
+
[source, text]
----
docker ps
CONTAINER ID        IMAGE                  COMMAND                CREATED             STATUS              PORTS                                              NAMES
f21dba8846bc        jboss/wildfly:latest   "/opt/jboss/wildfly/   10 minutes ago      Up 4 minutes        0.0.0.0:49161->8080/tcp, 0.0.0.0:49162->9990/tcp   desperate_sammet
----
+
Port `8080` is mapped to `49161` and port `9990` is mapped to `49162`.
. Verify your IP using `bootdocker ip` and access the default web page and admin console on these ports.
. WildFly require a user in administration realm before admin console can be accessed. This require a Dockerfile to be created which would create that user. Since we are creating a new Dockerfile, we have two options:
+
.. Just create the user and bind network interfaces on command-line
.. Create the user and override CMD to bind all network interfaces. This will keep command-line simple.
+

##Run the image
+
[source, text]
----
docker run -P -d hmesha/wildfly10
----
+
. Check the mapped ports using `docker ps`. Access the admin console at the mapped port and use ``admin'' username and ``admin'' password.