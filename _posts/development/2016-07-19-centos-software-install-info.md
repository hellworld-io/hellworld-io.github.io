---
layout: post
title:  "CentOS Software Install Info"
date:   2016-07-19
type: development
category: development
tag: java dev
---

# CentOS 6.X

## Apache
* yum install
``` bash
	yum -y install httpd
```
* start
``` bash
	service httpd start
```
* version check
``` bash
	httpd -v
```
* conf files
``` bash
	/etc/httpd/conf/httpd.conf
```
* firewall open

``` bash
	1. vi /etc/sysconfig/iptables
	2. -A INPUT -s 103.60.126.254/32 -m state --state NEW -j ACCEPT COMMIT
	3. -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
	4. service iptables restart
```

## Java
``` bash
	1. yum list | grep java
	2. yum -y install java-1.6.0-openjdk.x86_64
	3. java -version
	4. vi /etc/profile
	5. Insert export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk.x86_64
	6. Insert export PATH=$PATH:$JAVA_HOME/bin
	7. source /etc/profile
```

## Tomcat
* Go to [Tomcat Download Site](http://tomcat.apache.org/download-80.cgi)
* Copy Binary Distributions link url
	* Example ![Example]({{ site.url}}/assets/img/postImg/copy_tomcat_binary_url.png)
* wget binary file

``` bash
	wget http://mirrors.koehn.com/apache/tomcat/tomcat-8/v8.0.5/bin/apache-tomcat-8.0.5.tar.gz
```

* unzip tar.gz

``` bash
	tar -zxf /tmp/apache-tomcat-8.0.5.tar.gz -C /opt/tomcat
```

* Create User

``` bash
	$ groupadd tomcat
    $ useradd -g tomcat tomcat
    $ passwd tomcat
```

* Add Tomcat8 Service

``` bash
	1. vi /etc/init.d/tomcat8
	2. Insert
		# chkconfig: 345 90 90
        # description: init file for tomcat
        # processname: tomcat
        CATALINA_HOME=/opt/tomcat; export CATALINA_HOME
		JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk.x86_64; export JAVA_HOME

        start() {
            echo -n "starting Tomcat: "
            $CATALINA_HOME/bin/startup.sh
        }

        stop() {
            echo -n "stopping Tomcat: "
            $CATALINA_HOME/bin/shutdown.sh
        }

        case "$1" in
        start)
            start
            ;;
        stop)
            stop
            ;;
        restart)
            stop
            start
            ;;
        *)
            echo $"Usage: tomcat {start|stop|restart}"
            exit
        esac
    3. Do
		chmod +x /etc/init.d/tomcat8
		chkconfig --add tomcat8
		chkconfig --level 2345 tomcat8 on
		service tomcat8 start
```

* Create Tomcat User for tomcat manager

``` bash
	1. vi /opt/tomcat/conf/tomcat-users.xml
	2. Insert in tomcat-users tag
		<role rolename="manager-gui"/>
		<user username="USERNAME" password="PASSWORD" roles="manager-gui"/>
```

## FTP
``` bash
	1. yum install vsftpd
	2. vi /etc/vsftpd/vsftpd.conf
		- anoymous_enable=NO
    3. service iptables restart
    4. vi /etc/sysconfig/iptables
	    - Insert
	    	-A INPUT -m state --state NEW -m tcp -p tcp --dport 20 -j ACCEPT
			-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
    5. service iptables restart
    6. service vsftpd start
    7. chkconfig --level 2345 vsftpd on
    8. process check ==> ps -ef | grep vsftpd netstat -ntlp
```


