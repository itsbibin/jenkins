FROM tomcat:8.0-alpine 

LABEL maintainer="bibin@bibin.com"

ADD hello-2.0.war /usr/local/tomcat/webapps/

EXPOSE 8080

CMD ["catalina.sh", "run"]
