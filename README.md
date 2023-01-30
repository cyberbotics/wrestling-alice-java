# Humanoid Robot Wrestling Controller Example

[![webots.cloud - Competition](https://img.shields.io/badge/webots.cloud-Competition-007ACC)][1]

## Alice Java controller

Minimalist Java controller example for the [Humanoid Robot Wrestling Competition](https://github.com/cyberbotics/wrestling).
Demonstrates how to play a simple motion file. We use the [Motion class](https://cyberbotics.com/doc/reference/motion?tab-language=java) from Webots.

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

[Bob](https://github.com/cyberbotics/wrestling-bob) is a more advanced robot controller able to win against Alice.

[1]: https://webots.cloud/run?version=R2022b&url=https%3A%2F%2Fgithub.com%2Fcyberbotics%2Fwrestling%2Fblob%2Fmain%2Fworlds%2Fwrestling.wbt&type=competition "Leaderboard"
