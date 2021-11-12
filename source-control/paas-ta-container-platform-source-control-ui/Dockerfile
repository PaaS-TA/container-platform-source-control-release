FROM openjdk:8-jdk-alpine
ARG WAR_FILE=./*.war
COPY ${WAR_FILE} container-platform-source-control-ui.war
COPY application.properties /application.properties
ENTRYPOINT ["java","-jar","-Dspring.config.location=application.properties","/container-platform-source-control-ui.war"]
