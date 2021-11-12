FROM openjdk:8-jdk-alpine
ARG SCM_FILE=./*.tar.gz
COPY ${SCM_FILE} scm-server-app.tar.gz
RUN tar zxvf scm-server-app.tar.gz
RUN rm scm-server-app.tar.gz
ENTRYPOINT ["sh", "/scm-server/bin/scm-server"]
