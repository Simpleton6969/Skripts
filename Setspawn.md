In this tutorial we will create a simple spawn and setspawn with a 5 second delay between command completion and teleportation.
Including checking if the player has moved.
No requirements.

1) [The basics](#Skin-and-bones-commands)
2) [Variable checking](#Variable-checking)
3) [Cooldown and delay](#Cooldown-and-delay)
4) [Location checking](#Location-checking)
5) [Full code](#Full-code)

## Skin and bones commands
Lets get started by making the setspawn command
```vb
command /setspawn:
	permission: skript.setspawn # makes sure that only permitted users can execute this command
	permission message: "&cMissing &4skript.setspawn &cpermission" # lets you know what permission you are missing
	trigger:
		set {spawn} to player's location
		send "Spawn has been set to your location. &e/spawn"
```

```vb
command /spawn:
	trigger:
		teleport player to {spawn}
		send "You have been teleported to &eSpawn"
```
This does work but if the variable isnt set then the player wont get teleported, so lets check if the variable is set
## Variable checking
```vb
command /spawn:
	trigger:
		if {spawn} is set:
			teleport player to {spawn}
			send "You have been teleported to &eSpawn"
		else:
			send "&6Spawn &7is not set &6/setspawn"
```
## Cooldown and delay
Lets add a 5 second cooldown with a 5 second delay to deter players from using this to escape combat
```vb
command /spawn:
	cooldown: 5 seconds
	cooldown message: "You must wait &e%remaining time%"
	trigger:
		if {spawn} is set:
			loop 5 times:
				wait 1 second
				send "%5-loop-value%"
			teleport player to {spawn}
			send "You have been teleported to &eSpawn"
		else:
			send "&6Spawn &7is not set &6/setspawn"
```
## Location checking
Finally lets make it check the location of the player(give or take 0.25 blocks)
```vb
command /spawn:
	cooldown: 5 seconds
	cooldown message: "You must wait &e%remaining time%"
	trigger:
		if {spawn} is set:
			set {_loc} to player's location
			loop 5 times:
				wait 1 second
				if distance between {_loc} and player's location > 0.25:
					send "&cYou have moved, teleportation cancelled."
					stop
				else:
					send "%5-loop-value%"
			teleport player to {spawn}
			send "You have been teleported to &eSpawn"
		else:
			send "&6Spawn &7is not set &6/setspawn"
```
## Full code
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
		send "Spawn has been set to your location. &e/spawn"
```
