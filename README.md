# SonarQube

1.Criar a base de dados: 

CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER ‘sonar’ IDENTIFIED BY ‘sonar’;
GRANT ALL ON sonar.* TO ‘sonar’@’%’ IDENTIFIED BY ‘sonar’;
GRANT ALL ON sonar.* TO ‘sonar’@’localhost’ IDENTIFIED BY ‘sonar’;
FLUSH PRIVILEGES;

2. Configurar o banco de dados no arquivo "Sonar.Prorpeties": 

Arquivo: 
*******************************************************************************************************************************
# Property values can:
# - reference an environment variable, for example sonar.jdbc.url= ${env:SONAR_JDBC_URL}
# - be encrypted. See http://redirect.sonarsource.com/doc/settings-encryption.html

#--------------------------------------------------------------------------------------------------
# DATABASE
#
# IMPORTANT: the embedded H2 database is used by default. It is recommended for tests but not for
# production use Supported databases are MySQL, Oracle, PostgreSQL and Microsoft SQLServer.

# User credentials.
# Permissions to create tables, indices and triggers must be granted to JDBC user.
# The schema must be created first.
#----- Embedded Database (default)
# H2 embedded database server listening port, defaults to 9092
#sonar.embeddedDatabase.port=9092
#----- MySQL 5.6 or greater
# Only InnoDB storage engine is supported (not myISAM).
# Only the bundled driver is supported. It can not be changed.
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
sonar.jdbc.driverClassName=com.mysql.jdbc.Driver
sonar.jdbc.validationQuery=select 1
sonar.jdbc.username=root
sonar.jdbc.password=123456

#----- Oracle 11g/12c
# - Only thin client is supported
# - Only versions 11.2.x and 12.x of Oracle JDBC driver are supported
# - The JDBC driver must be copied into the directory extensions/jdbc-driver/oracle/
# - If you need to set the schema, please refer to http://jira.sonarsource.com/browse/SONAR-5000
#sonar.jdbc.url=jdbc:oracle:thin:@localhost:1521/XE


#----- PostgreSQL 8.x/9.x
# If you don't use the schema named "public", please refer to http://jira.sonarsource.com/browse/SONAR-5000
#sonar.jdbc.url=jdbc:postgresql://localhost/sonar


#----- Microsoft SQLServer 2008/2012/2014 and SQL Azure
# A database named sonar must exist and its collation must be case-sensitive (CS) and accent-sensitive (AS)
# Use the following connection string if you want to use integrated security with Microsoft Sql Server
# Do not set sonar.jdbc.username or sonar.jdbc.password property if you are using Integrated Security
# For Integrated Security to work, you have to download the Microsoft SQL JDBC driver package from
# http://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=11774
# and copy sqljdbc_auth.dll to your path. You have to copy the 32 bit or 64 bit version of the dll
# depending upon the architecture of your server machine.
# This version of SonarQube has been tested with Microsoft SQL JDBC version 4.1
#sonar.jdbc.url=jdbc:sqlserver://localhost;databaseName=sonar;integratedSecurity=true

# Use the following connection string if you want to use SQL Auth while connecting to MS Sql Server.
# Set the sonar.jdbc.username and sonar.jdbc.password appropriately.
#sonar.jdbc.url=jdbc:sqlserver://localhost;databaseName=sonar


#----- Connection pool settings
# The maximum number of active connections that can be allocated
# at the same time, or negative for no limit.
# The recommended value is 1.2 * max sizes of HTTP pools. For example if HTTP ports are
# enabled with default sizes (50, see property sonar.web.http.maxThreads)
# then sonar.jdbc.maxActive should be 1.2 * (50) = 120.
#sonar.jdbc.maxActive=60

# The maximum number of connections that can remain idle in the
# pool, without extra ones being released, or negative for no limit.
#sonar.jdbc.maxIdle=5

# The minimum number of connections that can remain idle in the pool,
# without extra ones being created, or zero to create none.
#sonar.jdbc.minIdle=2

# The maximum number of milliseconds that the pool will wait (when there
# are no available connections) for a connection to be returned before
# throwing an exception, or <= 0 to wait indefinitely.
#sonar.jdbc.maxWait=5000

#sonar.jdbc.minEvictableIdleTimeMillis=600000
#sonar.jdbc.timeBetweenEvictionRunsMillis=30000



#--------------------------------------------------------------------------------------------------
# WEB SERVER
# Web server is executed in a dedicated Java process. By default heap size is 512Mb.
# Use the following property to customize JVM options.
#    Recommendations:
#
#    The HotSpot Server VM is recommended. The property -server should be added if server mode
#    is not enabled by default on your environment:
#    http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html
#
#sonar.web.javaOpts=-Xmx512m -Xms128m -XX:+HeapDumpOnOutOfMemoryError -Djava.net.preferIPv4Stack=true

# Same as previous property, but allows to not repeat all other settings like -Xmx
#sonar.web.javaAdditionalOpts=

# Binding IP address. For servers with more than one IP address, this property specifies which
# address will be used for listening on the specified ports.
# By default, ports will be used on all IP addresses associated with the server.
#sonar.web.host=0.0.0.0

# Web context. When set, it must start with forward slash (for example /sonarqube).
# The default value is root context (empty value).
#sonar.web.context=
# TCP port for incoming HTTP connections. Default value is 9000.
#sonar.web.port=9000


# The maximum number of connections that the server will accept and process at any given time.
# When this number has been reached, the server will not accept any more connections until
# the number of connections falls below this value. The operating system may still accept connections
# based on the sonar.web.connections.acceptCount property. The default value is 50.
#sonar.web.http.maxThreads=50

# The minimum number of threads always kept running. The default value is 5.
#sonar.web.http.minThreads=5

# The maximum queue length for incoming connection requests when all possible request processing
# threads are in use. Any requests received when the queue is full will be refused.
# The default value is 25.
#sonar.web.http.acceptCount=25

# TCP port for incoming AJP connections. Disabled if value is -1. Disabled by default.
#sonar.ajp.port=-1


#--------------------------------------------------------------------------------------------------
# COMPUTE ENGINE
# The Compute Engine is responsible for processing background tasks.
# Compute Engine is executed in a dedicated Java process. Default heap size is 512Mb.
# Use the following property to customize JVM options.
#    Recommendations:
#
#    The HotSpot Server VM is recommended. The property -server should be added if server mode
#    is not enabled by default on your environment:
#    http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html
#
#sonar.ce.javaOpts=-Xmx512m -Xms128m -XX:+HeapDumpOnOutOfMemoryError -Djava.net.preferIPv4Stack=true

# Same as previous property, but allows to not repeat all other settings like -Xmx
#sonar.ce.javaAdditionalOpts=
# The number of workers in the Compute Engine. Value must be greater than zero.
# By default the Compute Engine uses a single worker and therefore processes tasks one at a time.
#    Recommendations:
#
#    Using N workers will require N times as much Heap memory (see property
#    sonar.ce.javaOpts to tune heap) and produce N times as much IOs on disk, database and
#    Elasticsearch. The number of workers must suit your environment.
#sonar.ce.workerCount=1


#--------------------------------------------------------------------------------------------------
# ELASTICSEARCH
# Elasticsearch is used to facilitate fast and accurate information retrieval.
# It is executed in a dedicated Java process. Default heap size is 1Gb.

# JVM options of Elasticsearch process
#    Recommendations:
#
#    Use HotSpot Server VM. The property -server should be added if server mode
#    is not enabled by default on your environment:
#    http://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html
#
#sonar.search.javaOpts=-Xmx1G -Xms256m -Xss256k -Djava.net.preferIPv4Stack=true \
#  -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:CMSInitiatingOccupancyFraction=75 \
#  -XX:+UseCMSInitiatingOccupancyOnly -XX:+HeapDumpOnOutOfMemoryError

# Same as previous property, but allows to not repeat all other settings like -Xmx
#sonar.search.javaAdditionalOpts=

# Elasticsearch port. Default is 9001. Use 0 to get a free port.
# As a security precaution, should be blocked by a firewall and not exposed to the Internet.
#sonar.search.port=9001

# Elasticsearch host. The search server will bind this address and the search client will connect to it.
# Default is 127.0.0.1.
# As a security precaution, should NOT be set to a publicly available address.
#sonar.search.host=127.0.0.1


#--------------------------------------------------------------------------------------------------
# UPDATE CENTER

# Update Center requires an internet connection to request https://update.sonarsource.org
# It is enabled by default.
#sonar.updatecenter.activate=true

# HTTP proxy (default none)
#http.proxyHost=
#http.proxyPort=
# HTTPS proxy (defaults are values of http.proxyHost and http.proxyPort)
#https.proxyHost=
#https.proxyPort=

# NT domain name if NTLM proxy is used
#http.auth.ntlm.domain=

# SOCKS proxy (default none)
#socksProxyHost=
#socksProxyPort=

# Proxy authentication (used for HTTP, HTTPS and SOCKS proxies)
#http.proxyUser=
#http.proxyPassword=


#--------------------------------------------------------------------------------------------------
# LOGGING

# Level of logs. Supported values are INFO(default), DEBUG and TRACE (DEBUG + SQL + ES requests)
#sonar.log.level=INFO

# Path to log files. Can be absolute or relative to installation directory.
# Default is <installation home>/logs
#sonar.path.logs=logs

# Rolling policy of log files
#    - based on time if value starts with "time:", for example by day ("time:yyyy-MM-dd")
#      or by month ("time:yyyy-MM")
#    - based on size if value starts with "size:", for example "size:10MB"
#    - disabled if value is "none".  That needs logs to be managed by an external system like logrotate.
#sonar.log.rollingPolicy=time:yyyy-MM-dd

# Maximum number of files to keep if a rolling policy is enabled.
#    - maximum value is 20 on size rolling policy
#    - unlimited on time rolling policy. Set to zero to disable old file purging.
#sonar.log.maxFiles=7

# Access log is the list of all the HTTP requests received by server. If enabled, it is stored
# in the file {sonar.path.logs}/access.log. This file follows the same rolling policy as for
# sonar.log (see sonar.log.rollingPolicy and sonar.log.maxFiles).
#sonar.web.accessLogs.enable=true

# Format of access log. It is ignored if sonar.web.accessLogs.enable=false. Possible values are:
#    - "common" is the Common Log Format, shortcut to: %h %l %u %user %date "%r" %s %b
#    - "combined" is another format widely recognized, shortcut to: %h %l %u [%t] "%r" %s %b "%i{Referer}" "%i{User-Agent}"
#    - else a custom pattern. See http://logback.qos.ch/manual/layouts.html#AccessPatternLayout.
# If SonarQube is behind a reverse proxy, then the following value allows to display the correct remote IP address:
#sonar.web.accessLogs.pattern=%i{X-Forwarded-For} %l %u [%t] "%r" %s %b "%i{Referer}" "%i{User-Agent}"
# Default value is:
#sonar.web.accessLogs.pattern=combined


#--------------------------------------------------------------------------------------------------
# OTHERS

# Delay in seconds between processing of notification queue. Default is 60 seconds.
#sonar.notifications.delay=60

# Paths to persistent data files (embedded database and search index) and temporary files.
# Can be absolute or relative to installation directory.
# Defaults are respectively <installation home>/data and <installation home>/temp
#sonar.path.data=data
#sonar.path.temp=temp


#--------------------------------------------------------------------------------------------------
# DEVELOPMENT - only for developers
# The following properties MUST NOT be used in production environments.

# Dev mode allows to reload web sources on changes and to restart server when new versions
# of plugins are deployed.
#sonar.web.dev=false

# Path to webapp sources for hot-reloading of Ruby on Rails, JS and CSS (only core,
# plugins not supported).
#sonar.web.dev.sources=/path/to/server/sonar-web/src/main/webapp

# Elasticsearch HTTP connector, for example for KOPF:
# http://lmenezes.com/elasticsearch-kopf/?location=http://localhost:9010
#sonar.search.httpPort=-1


*******************************************************************************************************************************


3.Logar no Sonar: 

Login: admin 
Senha: admin 

(default) 


