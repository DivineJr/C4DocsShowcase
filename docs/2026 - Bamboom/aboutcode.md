---
icon: lucide/code
---

# Bamboom's Code
Bamboom, like the majority of other robots, is written in Java using WPILib, the pretty much official, near required library used and supported by FIRST.

Bamboom uses a Command Based structure, a subset of a Timed structure, a decision made in order to support the development of an effective Autonomous and support by tools used in the creation of an Autonomous mode.

## What is Command Based?
Since Command Based is built on top of the older Timed structure, it's probably easiest to cover that first.

### Timed Structure
A Timed robot runs code every 0.050 seconds, 20 times a second. Any time spent running code longer than 0.050 seconds may cause warning or an outright crash of the robot code.

The timed robot, based on the mode selected by the Field Management System (FMS) or the Driver Station when outside of an event, runs code in the relevant sections of the robot.

#### Global Code
```java
public Robot() {
    A
}

public robotPeriodic() {
    B
}

public disabledInit() {
    C
}

public disabledPeriodic() {
    D
}

public teleopInit() {
    E
}

public teleopPeriodic() {
    F
}
```

The robot will at startup run the `Robot()` part of code, running all code in `A`, initializing parts of the code (it's up to the programmer to put necessary code in these sections). This will run only once.

After this, the robot every cycle will run the code put in `robotPeriodic()`, section `B`. This is regardless of what mode the robot is in, regardless of whether it's disabled, enabled, in teleop, or autonomous.

This is useful for code relating to LEDs or vision that you may want to run at all times.

#### Mode Based Code
The robot will also run code based on what mode it is in. By default the robot is **disabled**. While disabled, WPILib won't even allow you to communicate to motors or other moving parts of the robot, both for safety reasons and the rules of the competition.

Otherwise, similarly to the global parts of the code, when the robot is switched into disabled mode, either by starting up or being disabled at a later time, it will first run `disabledInit()`, running all code in section `C`. This will only run once per switch into disabled. This code will run every time the robot enters disabled mode.

Next, it will every cycle run the code in `teleopPeriodic()`, running the code in section `D`.

The same thing happens in teleop, running `teleopInit()` once per mode switch and proceeding to `teleopPeriodic()` every cycle.

Just to be clear, on the initial cycle of a mode switch, both the `Init()` and the `Periodic()` run on the same cycle. 

### Command Based Explanation

As mentioned before, Command Based is built on top of Timed, and when making a new Command Based robot code project, you will notice all the `Init()` and `Periodic()` methods are still there.

While not recommended, you can still code a robot in a similar way as to how you would a Timed, as it is still just a Timed robot.

---

In order to keep this accessible to the average person, we will be skimming on the details.

The main difference in a Command Based robot is that the **CommandScheduler** has been added, running code every cycle in `robotPeriodic()`. The point of this is that you can send **Commands** containing code to the CommandScheduler which will be run every cycle.

This may sound obtuse, but the point of this is that we can dynamically add and remove Commands, and therefore code we want to run, from the CommandScheduler. The main benefits of this are shown in Autonomous, allowing us to easily program instructions and paths for the robot.

The CommandScheduler does also introduce more complexity to the robot as a whole, but making anything Autonomous related easier (typically the hardest part of robot programming) is a fair tradeoff to most teams.
