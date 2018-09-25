






# Tomcat 


From `jws-3.0/tomcat7/bin/startup.sh` :
```
# JON/Jopr Management
#
# To enable EWS for JON/Jopr management it is necessary to enable remote JMX access to the EWS server:
# 1) Choose an unused and accessible portNum
# 2) Follow the steps for secure access as described here: http://java.sun.com/j2se/1.5.0/docs/guide/management/agent.html.
# 3) Update and uncomment the following lines:
#
# JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.port=<port>"
# JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.access.file=<access-fi e-path>"
# JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.password.file=<password -file-path>"
# JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.ssl=false"
# export JAVA_OPTS
```


- [UNSUITABLE] For local management (like with local jconsole using the same user, so it doesn't work with JON):
```
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote"
export JAVA_OPTS
```

- [WORKS, but even with localhost as an rmi hostname, it listens on all interfaces] Like jon storage :
```bash
JAVA_OPTS="${JAVA_OPTS} -Djava.rmi.server.hostname=localhost" # doesn't seem to work
#JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote=true" # superfluous
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.port=12345"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.ssl=false"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.authenticate=false"
export JAVA_OPTS
```

- [WORKS, but the password is in cleartext] auth w/o ssl
```bash
#! make sure the files are owned by the tomcat user and that they have 600 access mode 
JAVA_OPTS="${JAVA_OPTS} -Djava.rmi.server.hostname=localhost"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.port=12345"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.access.file=/etc/jon/tomcat/jmxremote.access"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.password.file=/etc/jon/tomcat/jmxremote.password"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.authenticate=true"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.ssl=false"
export JAVA_OPTS
```

- [WORKS] auth w/o ssl and listens only on localhost
```
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.host=localhost"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.port=12345"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.access.file=/etc/jon/tomcat/jmxremote.access"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.password.file=/etc/jon/tomcat/jmxremote.password"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.authenticate=true"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.ssl=false"
export JAVA_OPTS
```


For remote management:
```bash
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.port=12345"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.access.file=/etc/jon/tomcat/jmxremote.access"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.password.file=/etc/jon/tomcat/jmxremote.password"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.authenticate=true"
JAVA_OPTS="${JAVA_OPTS} -Dcom.sun.management.jmxremote.ssl=true"
# insert correct ssl setup
export JAVA_OPTS
```

## docs

- http://docs.oracle.com/javase/6/docs/technotes/guides/management/agent.html
- http://docs.oracle.com/javase/8/docs/technotes/guides/management/agent.html
- https://www.ibm.com/support/knowledgecenter/SSCP65_5.0.1/com.ibm.jazz.repository.web.admin.doc/topics/t_server_mon_tomcat_option2.html
- https://www.ibm.com/support/knowledgecenter/SSYMRC_6.0.1/com.ibm.jazz.repository.web.admin.doc/topics/t_server_mon_tomcat_option3.html
- https://www.lullabot.com/articles/monitor-java-with-jmx
- https://db.apache.org/derby/docs/10.9/adminguide/radminjmxenablepwdssl.html
