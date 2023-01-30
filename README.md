# Humanoid Robot Wrestling Controller Example

[![webots.cloud - Competition](https://img.shields.io/badge/webots.cloud-Competition-007ACC)][1]

## Alice Java controller

Minimalist Java controller example for the [Humanoid Robot Wrestling Competition](https://github.com/cyberbotics/wrestling).
Demonstrates how to play a simple motion file. We use the [Motion class](https://cyberbotics.com/doc/reference/motion?tab-language=java) from Webots.

Here is the [participant.java](./controllers/participant/participant.java) file:

``` Java
import com.cyberbotics.webots.controller.Robot;
import com.cyberbotics.webots.controller.Motion;

public class participant {
  public static void main(String[] args) {
    // Robot initialization
    Robot robot = new Robot();

    // Motion files are text files containing pre-recorded positions of the robot's joints
    Motion handWave = new Motion("../motions/HandWave.motion");
    // We play the hand-waving motion on loop
    handWave.setLoop(true);
    handWave.play();

    int timeStep = (int) Math.round(robot.getBasicTimeStep());
    // Mandatory function to make the simulation run
    while (robot.step(timeStep) != -1);
  }
}
```

To compile the Java code, the [Dockerfile](./controllers/Dockerfile) must be updated: we use the `eclipse-temurin:17-jdk` Docker image to get the latest OpenJDK 17 Java Development Kit installed on top of Webots's image. We compile `participant.java` into a class file and then we run it with the entrypoint command:

``` Dockerfile
FROM eclipse-temurin:17-jdk as source
FROM cyberbotics/webots.cloud:R2023a-ubuntu20.04
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
```

[Bob](https://github.com/cyberbotics/wrestling-bob) is a more advanced robot controller able to win against Alice.

[1]: https://webots.cloud/run?version=R2022b&url=https%3A%2F%2Fgithub.com%2Fcyberbotics%2Fwrestling%2Fblob%2Fmain%2Fworlds%2Fwrestling.wbt&type=competition "Leaderboard"
