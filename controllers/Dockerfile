FROM eclipse-temurin:17-jdk as source
FROM cyberbotics/webots:R2023b-ubuntu22.04
ENV JAVA_HOME=/opt/java/openjdk
ENV PATH "${JAVA_HOME}/bin:${PATH}"
COPY --from=source $JAVA_HOME $JAVA_HOME

# Environment variables needed for Webots
# https://cyberbotics.com/doc/guide/running-extern-robot-controllers#remote-extern-controllers
ENV LD_LIBRARY_PATH=${WEBOTS_HOME}/lib/controller:${LD_LIBRARY_PATH}
ARG WEBOTS_CONTROLLER_URL
ENV WEBOTS_CONTROLLER_URL=${WEBOTS_CONTROLLER_URL}

# Copies all the files of the controllers folder into the docker container
RUN mkdir -p /usr/local/webots-project/controllers
COPY . /usr/local/webots-project/controllers

WORKDIR /usr/local/webots-project/controllers/participant
# Build controller
RUN javac -Xlint -classpath /usr/local/webots/lib/controller/java/Controller.jar:. participant.java

ENTRYPOINT ["java", \
            "-classpath", "/usr/local/webots/lib/controller/java/Controller.jar:/usr/local/webots-project/controllers/participant", \
            "-Djava.library.path=/usr/local/webots/lib/controller/java", "participant"]
