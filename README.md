
---

# PLUGIN SYSTEM DOCUMENTATION (Made by Bladeee)

This document describes the structure and usage of the `game_tick_packet`, which contains the real-time game state in Rocket League. The packet includes information about the ball's position, player stats, team information, game status, and more. It also describes how to format a plugin, and create a plugin. It is fundementally RLBOT API.



---

## Table of Contents
- [Overview](#overview)
- [Key Fields in `game_tick_packet`](#key-fields-in-game_tick_packet)
  - [Ball Information (`game_ball`)](#ball-information-game_ball)
  - [Game Information (`game_info`)](#game-information-game_info)
  - [Player Information (`game_cars`)](#player-information-game_cars)
  - [Team Information (`teams`)](#team-information-teams)
  - [Boost Pads Information (`game_boosts`)](#boost-pads-information-game_boosts)
- [Controlling the Bot and Plugin Info](#controlling-the-bot-and-plugin-info)
- [Example Usage](#example-usage)

---

## Overview

The `game_tick_packet` contains all the real-time data about the current state of the game, including:
- Ball position and velocity
- Player states (position, velocity, rotation, etc.)
- Team information (score, team index)
- Boost pad status
- Game progress and status (time remaining, overtime status, etc.)

---

## Key Fields in `game_tick_packet`

### Ball Information (`game_ball`)

Contains the current state of the ball in the game.

- **`game_ball.physics.location.x`**: The X coordinate of the ball's position.
- **`game_ball.physics.location.y`**: The Y coordinate of the ball's position.
- **`game_ball.physics.location.z`**: The Z coordinate of the ball's position (height above the ground).
- **`game_ball.physics.velocity.x`**: The X velocity of the ball.
- **`game_ball.physics.velocity.y`**: The Y velocity of the ball.
- **`game_ball.physics.velocity.z`**: The Z velocity of the ball.
- **`game_ball.physics.rotation.pitch`**: The ball's pitch rotation.
- **`game_ball.physics.rotation.yaw`**: The ball's yaw rotation.
- **`game_ball.physics.rotation.roll`**: The ball's roll rotation.
- **`game_ball.physics.angular_velocity.x`**: The ball's angular velocity in the X direction.
- **`game_ball.physics.angular_velocity.y`**: The ball's angular velocity in the Y direction.
- **`game_ball.physics.angular_velocity.z`**: The ball's angular velocity in the Z direction.

---

### Game Information (`game_info`)

Contains general information about the current game state.

- **`game_info.seconds_elapsed`**: The total number of seconds elapsed since the game started.
- **`game_info.game_time_remaining`**: The remaining time in the game (in seconds).
- **`game_info.game_speed`**: The current speed of the game (usually 1.0 under normal conditions).
- **`game_info.is_overtime`**: Boolean indicating whether the game is in overtime.
- **`game_info.is_round_active`**: Boolean indicating if the current round is active.
- **`game_info.is_unlimited_time`**: Boolean indicating whether the match has unlimited time (True for Rumble or other special modes).
- **`game_info.is_match_ended`**: Boolean indicating if the match has ended.
- **`game_info.world_gravity_z`**: The gravitational force in the Z direction (usually 1.0).
- **`game_info.is_kickoff_pause`**: Boolean indicating if the game is in a kickoff pause.
- **`game_info.frame_num`**: The current frame number of the game.

---

### Player Information (`game_cars`)

Contains data about each player’s car, such as their position, velocity, rotation, and more. Each player is represented by a `PlayerInfo` object.

- **`game_cars[<index>].team`**: The team index of the player (usually 0 for the first team and 1 for the second team).
- **`game_cars[<index>].physics.location.x`**: The X coordinate of the player's car.
- **`game_cars[<index>].physics.location.y`**: The Y coordinate of the player's car.
- **`game_cars[<index>].physics.location.z`**: The Z coordinate of the player's car.
- **`game_cars[<index>].physics.velocity.x`**: The X velocity of the player's car.
- **`game_cars[<index>].physics.velocity.y`**: The Y velocity of the player's car.
- **`game_cars[<index>].physics.velocity.z`**: The Z velocity of the player's car.
- **`game_cars[<index>].physics.rotation.pitch`**: The pitch rotation of the player's car.
- **`game_cars[<index>].physics.rotation.yaw`**: The yaw rotation of the player's car.
- **`game_cars[<index>].physics.rotation.roll`**: The roll rotation of the player's car.
- **`game_cars[<index>].physics.angular_velocity.x`**: The angular velocity in the X direction of the player's car.
- **`game_cars[<index>].physics.angular_velocity.y`**: The angular velocity in the Y direction of the player's car.
- **`game_cars[<index>].physics.angular_velocity.z`**: The angular velocity in the Z direction of the player's car.
- **`game_cars[<index>].has_wheel_contact`**: Boolean indicating whether the player’s car has wheel contact (on the ground).
- **`game_cars[<index>].is_super_sonic`**: Boolean indicating whether the player’s car is supersonic.
- **`game_cars[<index>].double_jumped`**: Boolean indicating whether the player has double jumped.
- **`game_cars[<index>].jumped`**: Boolean indicating whether the player has jumped.
- **`game_cars[<index>].boost`**: The current boost amount of the player's car (percentage from 0 to 100).
- **`game_cars[<index>].name`**: The player's name.

---

### Team Information (`teams`)

Contains information about the teams, such as their score.

- **`teams[<index>].score`**: The score of the team.
- **`teams[<index>].team_index`**: The team index (0 or 1).

---

### Boost Pads Information (`game_boosts`)

Contains information about each boost pad on the field.

- **`game_boosts[<index>].is_active`**: Boolean indicating whether the boost pad is active.
- **`game_boosts[<index>].timer`**: The time remaining for the boost pad to respawn. If inactive, the timer will show the remaining respawn time, otherwise, it will be set to 0.

---

## Example Usage

```python
# -- Some Example Usages of the API.


# self.player_index would be our up to date index supplied by the API, however you can choose any.
# Accessing the player's car location (X, Y, Z)
car_location_x = game_tick_packet.game_cars[player_index].physics.location.x
car_location_y = game_tick_packet.game_cars[player_index].physics.location.y
car_location_z = game_tick_packet.game_cars[player_index].physics.location.z

self.ConsoleLogger(f"Player's car location: ({car_location_x}, {car_location_y}, {car_location_z})")


# Assuming player_index is the index of the player whose team you want to check
player_index = 0  # self.player_index would be our local player index. 

# Get the team of the player by looking up their team index in game_cars
player_team_index = game_tick_packet.game_cars[player_index].team

# Get team info (team score, etc.)
team_info = game_tick_packet.teams[player_team_index]
self.ConsoleLogger(f"Player's team index: {player_team_index}")
self.ConsoleLogger(f"Team score: {team_info.score}")



# Accessing the game tick packet to print ball location
self.ConsoleLogger("Ball location: X =", game_tick_packet.game_ball.physics.location.x,
      "Y =", game_tick_packet.game_ball.physics.location.y,
      "Z =", game_tick_packet.game_ball.physics.location.z)

# Checking if the game has ended
if game_tick_packet.game_info.is_match_ended:
    self.ConsoleLogger("The match has ended.")

# Accessing the first player's information (car)
first_player = game_tick_packet.game_cars[0]
self.ConsoleLogger("First player name:", first_player.name)
self.ConsoleLogger("Boost amount:", first_player.boost)
self.ConsoleLogger("Car position:", first_player.physics.location.x, first_player.physics.location.y, first_player.physics.location.z)
```
---

## Example of Using Location Data
You might want to determine if your car is close to the ball or check its position relative to the goal. Here's an example:

```python
# Assuming you have the ball's position as well:
ball_location_x = game_tick_packet.game_ball.physics.location.x
ball_location_y = game_tick_packet.game_ball.physics.location.y
ball_location_z = game_tick_packet.game_ball.physics.location.z

# Calculate the distance between the player's car and the ball
distance_to_ball = ((car_location_x - ball_location_x) ** 2 + 
                    (car_location_y - ball_location_y) ** 2 + 
                    (car_location_z - ball_location_z) ** 2) ** 0.5

self.ConsoleLogger(f"Distance to the ball: {distance_to_ball} units")
```
---

---

## Controlling the bot and Plugin Info
Here is the documentation for the format of the plugin system, how it works, and some basic examples.
The plugin API provides a recent game tick packet every tick, along with an up-to-date local_player_index and player name. The main() function of your plugin will be called constantly, which is why a time.sleep() is often added to reduce CPU usage or to handle tasks that don’t require real-time precision, such as checking only every few seconds.

When creating your own plugin, you must name the class with "plugin_" as the prefix.
Here’s an example of printing the ball’s location, along with the required name setup and format:

```python
import time
from rlbot.agents.base_agent import SimpleControllerState

class plugin_Get_Ball_xyz:
    def __init__(self, player_index=0, ConsoleLogger=None):
        self.controller = None
        self.player_index = player_index
        self.game_tick_packet = None  # Initialize game_tick_packet to None
        self.ConsoleLogger = ConsoleLogger

    def Name(self):
        self.ConsoleLogger("Ball location print")
    
    def Description(self):
        self.ConsoleLogger("Constantly prints the ball location!")
    
    def Author(self):
        self.ConsoleLogger("Created by User782")
    
    def Version(self):
        self.ConsoleLogger("V1.1")

    def game_tick_packet_set(self, packet, local_player_index=0, playername="default", process_id=None):
        self.game_tick_packet = packet
        self.player_index = local_player_index
        self.pid = process_id
        self.playername = playername

        if self.controller:
            return self.controller
        else:
            return None
        
    def main(self):
        target_fps = 120  
        frame_duration = 1.0 / target_fps 
        while True:
            try:
                start_time = time.time()
                if self.game_tick_packet:
                    time.sleep(2)
                    ball_x_y_Z = f"Ball Location = X {self.game_tick_packet.game_ball.physics.location.x} y = {self.game_tick_packet.game_ball.physics.location.y} z = {self.game_tick_packet.game_ball.physics.location.z}"
                    self.ConsoleLogger(ball_x_y_Z)
                # Enforce the frame rate timing to sync the game ticks
                elapsed_time = time.time() - start_time
                sleep_duration = frame_duration - elapsed_time
                if sleep_duration > 0:
                    time.sleep(sleep_duration)

            except Exception as e:
                print(f"Error: {e}")
                time.sleep(5)
                self.controller = None
                pass
```

Controlling the Bot

You can control your bot's movements using the SimpleControllerState. This allows you to set actions such as moving forward, steering, jumping, and boosting.

The main() function of your plugin will be called constantly, however  your game_tick_packet_set function will be called every tick. This ensures that up-to-date game data and information are provided on every tick.

When creating a plugin and needing to return your own custom controller state, the SimpleControllerState is used.

```python
import time
from rlbot.agents.base_agent import SimpleControllerState

class plugin_Get_Ball_xyz:
    def __init__(self, player_index=0):
        self.controller = None
        self.player_index = player_index
        self.game_tick_packet = None  # Initialize game_tick_packet to None
        self.ConsoleLogger = ConsoleLogger # Initialize ConsoleLogger variable to print to the gui console

    def Name(self):
        self.ConsoleLogger "Ball location print"
    
    def Description(self):
        self.ConsoleLogger "Constantly prints the ball location!"
    
    def Author(self):
        self.ConsoleLogger "Created by User782"
    
    def Version(self):
        self.ConsoleLogger "V1.1"

    def game_tick_packet_set(self, packet, local_player_index=0, playername="default", process_id=None):
        self.game_tick_packet = packet
        self.player_index = local_player_index
        self.pid = process_id
        self.playername = playername

        if self.controller:
            return self.controller
        else:
            return None
```

This allows us to return a controller state of our own which will overide the bot. Meaning our controller state will be written into memory instead of the bots.
Returning None will tell our bot to continue running instead of writing our custom controller state.

---
## Using SimpleControllerState

The SimpleControllerState class provides a friendly interface for controlling the bot. This class wraps around the input controls that are sent to the bot for each game tick. It allows you to easily set the bot's behavior (such as jumping, boosting, etc)

Attributes:

    steer: Controls the steering of the car.
        Range: -1 (left) to 1 (right)

    throttle: Controls the car's throttle (acceleration).
        Range: -1 (backward) to 1 (forward)

    pitch: Controls the pitch (nose-up or nose-down).
        Range: -1 (nose-down) to 1 (nose-up)

    yaw: Controls the yaw (nose-left or nose-right).
        Range: -1 (nose-left) to 1 (nose-right)

    roll: Controls the roll (clockwise or anticlockwise rotation).
        Range: -1 (anticlockwise) to 1 (clockwise)

    jump: Controls whether the bot is jumping (True/False).

    boost: Controls whether the bot is boosting (True/False).

    handbrake: Controls whether the bot is using the handbrake (True/False).

    use_item: Controls whether the bot is using an item (for rumble mode, True/False).
    
Example:
```python
from rlbot.agents.base_agent import SimpleControllerState

# Create a SimpleControllerState instance
controller = SimpleControllerState(
    steer=0.5,      # Steer right
    throttle=1.0,   # Full throttle (accelerating forward)
    jump=True,      # Jumping
    boost=False,    # Not boosting
    handbrake=False # Not using the handbrake
)
self.controller = controller

```
In the example above:

Steer is set to 0.5, meaning the car will steer to the right at half intensity.
Throttle is set to 1.0, meaning the car is accelerating forward at full speed.
Jump is set to True, meaning the bot will jump on this tick.
Boost and Handbrake are set to False, so the bot will not boost or use the handbrake.

Then in game_tick_packet_set, because we have set self.controller, it will write it into memory instead of the current bot running output. So make sure to set self.controller back to None in order for the bot to then continue running after you have finished writing whatever movements you needed. You can do this by simple checks. 

---

---
---
## Combining everything together. (Creating a basic plugin)
This plugin demonstrates how to create a simple bot plugin that jumps only when its wheels are on the ground. The bot uses the `SimpleControllerState` to control its jumping behavior.

Returning a `controller_state` will override the bot's controlls in favour of ours. Return `None` to allow the bot to continue running as normal.
```python

import time
from rlbot.agents.base_agent import SimpleControllerState

class plugin_Jumping:
    def __init__(self, player_index=0, ConsoleLogger=None):
        self.controller = None
        self.player_index = player_index
        self.game_tick_packet = None
        self.ConsoleLogger = ConsoleLogger # Initialize ConsoleLogger variable to print to the gui console

    def Name(self):
        return "Plugin Jumping"
    
    def Description(self):
        return "Jumps whenever the car's wheels are on the ground!"
    
    def Author(self):
        return "Created by skiffy"
    
    def Version(self):
        return "V1.0"

    def game_tick_packet_set(self, packet, local_player_index=0, playername="default", process_id=None):
        self.game_tick_packet = packet
        self.player_index = local_player_index
        self.pid = process_id
        self.playername = playername
        #Here we overide the controls with our custom state, or return None to let the play.
        if self.controller: 
            return self.controller
        else:
            return None

    def main(self):
        while True:
            try:
                if self.game_tick_packet:
                    controller = SimpleControllerState()
                    
                    # Get the current local player's car (ours)
                    player_car = self.game_tick_packet.game_cars[self.player_index]
                    
                    # Jump if wheels are in contact with the ground
                    if player_car.has_wheel_contact:
                        controller.jump = True
                        self.controller = controller #When conditions are met, we overide the bots controls with ours! 
                    else:
                        self.controller = None #Let's the bot continue to write to memory instead of our custom controller state

                # Small sleep to prevent high CPU usage
                time.sleep(1 / 120)

            except Exception as e:
                self.ConsoleLogger(f"Error: {e}")
                time.sleep(5)
                self.controller = None
```



---

---
