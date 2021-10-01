# PaaS-TA 컨테이너 플랫폼 소스 컨트롤 이미지 빌드
## 소개
컨테이너 플랫폼 소스 컨트롤 프로젝트를 빌드하고 이미지를 생성하여 레파지토리에 이미지를 업로드하는 방법에 대해 기술하였다.
### 사전 설치
- JDK 설치
- MVN 설치
- Docker 설치

<br>

### 소스 컨트롤 빌드 방법
- 컨테이너 플랫폼 소스 컨트롤 프로젝트의 jar 파일 생성을 위해 빌드를 진행한다.
```
$ mvn package
```

<br>

### 소스 컨트롤 이미지 생성 방법
- 컨테이너 플랫폼 소스 컨트롤 프로젝트 이미지 생성에 필요한 파일을 동일한 디렉토리 내에 위치 시킨다.
- 파일 위치 : <br>
  + 컨테이너 플랫폼 소스 컨트롤
      - [paas-ta-container-platform-source-control-api](source-control/paas-ta-container-platform-source-control-api)
      - [paas-ta-container-platform-source-control-ui](source-control/paas-ta-container-platform-source-control-ui)
      - [paas-ta-container-platform-source-control-broker](source-control/paas-ta-container-platform-source-control-broker)
      

<br>

> 'container-platform-source-control-api'를 예시로 진행한다.

- 파일 디렉토리 구성
```
├── Dockerfile
├── application.properties
└── container-platform-source-control-api.jar
```
- Dockerfile 확인
```
$ cat Dockerfile
```
```
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} container-platform-source-control-api.jar
COPY application.properties /application.properties
ENTRYPOINT ["java","-jar","-Dspring.config.location=application.properties","-Dspring.profiles.active=prod","/container-platform-source-control-api.jar"]
```
- 이미지 생성
  + {HAProxy_IP} : BOSH Inception에 배포된 Deployment <b>'container-platform'</b> 의 haproxy public ip 입력
```
$ sudo docker build --tag {HAProxy_IP}:5001/container-platform/container-platform-source-control-api:latest .
```
- 이미지 생성 확인
```
$ sudo docker images

REPOSITORY                                                            TAG                 IMAGE ID            CREATED             SIZE
xx.xxx.xxx.xx:5001/container-platform/container-platform-source-control-api          latest              45918a869bfd        38 seconds ago      140MB
```

<br>

### 소스 컨트롤 이미지 Private Repository 업로드 방법
> Docker를 기준으로 진행한다.

- Docker daemon.json 파일 내 insecure-registries 설정에 Private Repository Url 추가 후 Docker를 재시작한다.
```
$ sudo vi /etc/docker/daemon.json
{
        "insecure-registries": ["{HAProxy_IP}:5001"]
}

# docker restart
$ sudo systemctl restart docker
```

- Bosh로 배포한 Private Repository에 로그인을 진행한다.
```
$ sudo docker login http://{HAProxy_IP}:5001 --username admin --password admin
```

- 로그인한 Private Repository에 컨테이너 플랫폼 이미지를 Push한다.
```
$ sudo docker push {HAProxy_IP}:5001/container-platform/container-platform-source-control-api:latest
```
- Private Repository에 이미지가 정상적으로 업로드 되었는지 확인한다.
```
$ curl -H 'Authorization:Basic YWRtaW46YWRtaW4=' http://{HAProxy_IP}:5001/v2/_catalog

{"repositories":["container-platform-source-control-api"]}
`````