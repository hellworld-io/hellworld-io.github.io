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
	* ```
		yum -y install httpd
      ```
* start
	* ```
		service httpd start
      ```
* version check
	* ```
		httpd -v
      ```
* conf files
	* ```
		/etc/httpd/conf/httpd.conf
      ```
* firewall open
	* ```
		vi /etc/sysconfig/iptables
        After -A INPUT -s 103.60.126.254/32 -m state --state NEW -j ACCEPT
COMMIT
		Insert -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
        Then service iptables restart
	  ```

## Java
* ```
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
	* ![Example ]({{ site.url}}/assets/img/postImg/copy_tomcat_binary_url.png)

