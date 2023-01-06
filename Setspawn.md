In this tutorial we will create a simple spawn and setspawn with a 5 second delay between command completion and teleportation.
Including checking if the player has moved.
No requirements.

Lets get started by making the setspawn command
```
command /setspawn:
	permission: skript.setspawn # makes sure that only permitted users can execute this command
	permission message: "&cMissing &4skript.setspawn &cpermission" # lets you know what permission you are missing
	trigger:
		set {spawn} to player's location
		send "Spawn has been set to your location. &e/spawn"
```
		
Full code without comments:
```command /spawn:
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
