FROM openjdk:8-jdk-alpine
ARG JAR_FILE=./*.jar
COPY ${JAR_FILE} paas-ta-container-platform-source-control-broker.jar
COPY application.properties /application.properties
ENTRYPOINT ["java","-jar","-Dspring.config.location=application.properties","/paas-ta-container-platform-source-control-broker.jar"]
