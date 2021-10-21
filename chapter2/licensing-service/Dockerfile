#DOCKER FILE USING LAYERS
FROM openjdk:11-slim as build
WORKDIR application
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM openjdk:11-slim
WORKDIR application
#Copies each layer displayed as a result of the jarmode command
COPY --from=build application/dependencies/ ./
COPY --from=build application/spring-boot-loader/ ./
COPY --from=build application/snapshot-dependencies/ ./
COPY --from=build application/application/ ./
#Uses org.springframework.boot.loader.JarLauncher to execute the application
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
#DOCKER FILE USING LAYERS

#
#
#
#

#BASIC DOCKER FILE

#Start with a base image containing Java runtime
FROM openjdk:11-slim

# Add Maintainer Info
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's jar file
ARG JAR_FILE

# Add the application's jar to the container USE THE  <JAR_FILE> inside <buildArgs>
COPY ${JAR_FILE} app.jar

#execute the application
ENTRYPOINT ["java","-jar","/app.jar"]

#BASIC DOCKER FILE

#
#
#
#

#MULTISTAGE ADDS ONLY THE NECESSARY THINGS TO RUN THE APPLICATION

##stage 1
##Start with a base image containing Java runtime
#FROM openjdk:11-slim as build
#
## Add Maintainer Info
#LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"
#
## The application's jar file
#ARG JAR_FILE
#
## Add the application's jar to the container
#COPY ${JAR_FILE} app.jar
#
##unpackage jar file
#RUN mkdir -p target/dependency &&
#    (cd target/dependency; jar -xf /app.jar)
#
##stage 2
##Same Java runtime
#FROM openjdk:11-slim
#
##Add volume pointing to /tmp
#VOLUME /tmp
#
##Copy unpackaged application to new container
#ARG DEPENDENCY=/target/dependency
#COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
#COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
#COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
#
##execute the application
#ENTRYPOINT ["java","-cp","app:app/lib/*","com.optimagrowth.license.
#            LicenseServiceApplication"]

#MULTISTAGE ADDS ONLY THE NECESSARY THINGS TO RUN THE APPLICATION