Apache Syncope - Debian Packager
================================

#### Generates a debian package for installing Apache Syncope application with Apache Tomcat 7 servlet container.

* Build the project:
~~~
$ mvn clean package
~~~
which configures the default console credentials (admin/password), or
~~~
$ mvn clean package -Dsecurity.username=username -Dsecurity.password=[SHA1(password)]  
~~~
to change them (replace [SHA1(password)] with the SHA1 encryption of your password).

* Configure you java environment in /etc/default/tomcat7:
~~~
JAVA_OPTS="-Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms1536m -Xmx1536m -XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=256m -XX:MaxPermSize=256m -XX:+DisableExplicitGC"
~~~

* Uncomment 
~~~
<Manager pathname="" /> 
~~~
and  add
~~~ 
<Resource name="jdbc/syncopeDataSource" auth="Container" type="javax.sql.DataSource" 
                factory="org.apache.tomcat.jdbc.pool.DataSourceFactory" testWhileIdle="true" 
                testOnBorrow="true" testOnReturn="true" validationQuery="SELECT 1" validationInterval="30000" 
                maxActive="100" minIdle="2" maxWait="10000" initialSize="2" removeAbandonedTimeout="20000" 
                removeAbandoned="true" logAbandoned="true" suspectTimeout="20000" timeBetweenEvictionRunsMillis="5000" 
                minEvictableIdleTimeMillis="5000" 
                jdbcInterceptors="org.apache.tomcat.jdbc.pool.interceptor.ConnectionState;org.apache.tomcat.jdbc.pool.interceptor.StatementFinalizer" 
                username="syncope" password="password" driverClassName="com.postgresql.Driver" 
                url="jdbc:postgresql://localhost:5432/syncope?characterEncoding=UTF-8"/>
~~~
in $CATALINA_HOME/conf/context.xml.

* Install Apache Syncope using the maven-generated debian package in ${project.build.directory}
~~~
sudo dpkg -i apache-syncope-${syncope.version}~debv${project-version}.deb
~~~

* You can now access the web interface at 
~~~
http://localhost:8080/console
~~~
