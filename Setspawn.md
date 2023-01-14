In this tutorial we will create a simple spawn and setspawn with a 5 second delay between command completion and teleportation.
Including checking if the player has moved.
No requirements.

Lets get started by making the setspawn command
```vb
command /setspawn:
	permission: skript.setspawn # makes sure that only permitted users can execute this command
	permission message: "&cMissing &4skript.setspawn &cpermission" # lets you know what permission you are missing
	trigger:
		set {spawn} to player's location
		send "Spawn has been set to your location. &e/spawn"
```
This is as simple as setting a variable to the executors location.

As for the spawn command we want to make a 5 second delay between command execution and teleporting.
So we will create a 5 times loop and compare the players location to the players location upon command completion.
```vb
command /spawn:
	cooldown: 5 seconds # adds a 5 second cooldown to the command
	cooldown message: "You must wait &e%remaining time%" # shows remaining time between commands
	trigger:
		if {spawn} is set: # check if the spawn variable is set so the player has somewhere to teleport to.
			set {_loc} to player's location # setting a variable to the player location to compare later
			send "Stand still for 5 seconds to be teleported"
			send "&a5"
			set {_t} to 5
			loop 5 times: # creating a 5 second loop by looping 5 times and adding a 1 second wait
				if distance between player's location and {_loc} > 0.25: # compare players location with location on command
					send "&cTeleportation cancelled"
					stop # this will stop the code if the players location does not match their location on command
				else:
					wait 1 second # makes a 5 times loop a 5 second loop
					remove 1 from {_t}
					if {_t} > 0: # we dont want to send the 0 to player
						send "&a%{_t}%" # sends the seconds remaining to the player
			teleport player to {spawn}
			send "&2Teleport successful, welcome to spawn."
		else:
			send "Spawn is not set. &e/setspawn &rto set"
```

Full code without comments:
```vb
command /spawn:
	cooldown: 5 seconds
	cooldown message: "You must wait &e%remaining time%"
	trigger:
		if {spawn} is set:
			set {_loc} to player's location
			send "Stand still for 5 seconds to be teleported"
			send "&a5"
			set {_t} to 5
			loop 5 times:
				if player's location is not {_loc}:
					send "&cTeleportation cancelled"
					stop
				else:
					wait 1 second
					remove 1 from {_t}
					if {_t} > 0:
						send "&a%{_t}%"
			teleport player to {spawn}
			send "&2Teleport successful, welcome to spawn."
		else:
			send "Spawn is not set. &e/setspawn &rto set"
command /setspawn:
	permission: skript.setspawn
	permission message: "&cMissing &4skript.setspawn &cpermission"
	trigger:
		set {spawn} to player's location
		send "Spawn has been set to your location. &e/spawn"```
