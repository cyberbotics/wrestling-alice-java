FROM eclipse-temurin:17-jdk as source
FROM cyberbotics/webots.cloud:R2023a-ubuntu20.04
ENV JAVA_HOME=/opt/java/openjdk
ENV PATH "${JAVA_HOME}/bin:${PATH}"
COPY --from=source $JAVA_HOME $JAVA_HOME

# Copies all the files of the controllers folder into the docker container
RUN mkdir -p /usr/local/webots-project/controllers
COPY . /usr/local/webots-project/controllers

WORKDIR /usr/local/webots-project/controllers/participant
# Build controller
RUN javac -Xlint -classpath /usr/local/webots/lib/controller/java/Controller.jar:. participant.java

ENTRYPOINT ["java", \
            "-classpath", "/usr/local/webots/lib/controller/java/Controller.jar:/usr/local/webots-project/controllers/participant", \
            "-Djava.library.path=/usr/local/webots/lib/controller/java", "participant"]
