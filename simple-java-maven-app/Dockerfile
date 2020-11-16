FROM openjdk:11-jdk-slim
WORKDIR /home/demo
ARG REVISION
COPY target/my-app-1.0-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]