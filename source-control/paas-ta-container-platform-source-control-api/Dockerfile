FROM openjdk:8-jdk-alpine
ARG JAR_FILE=./*.jar
COPY ${JAR_FILE} container-platform-source-control-api.jar
COPY application.properties /application.properties
ENTRYPOINT ["java","-jar","-Dspring.config.location=application.properties","/container-platform-source-control-api.jar"]