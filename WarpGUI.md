In this skript we will be making a functional setwarp and warp command.
Whereby the player can input an argument to the warp command to skip the GUI.
Or leave the argument and open a gui which they can click out of all the warps.
For this we will be using yml rather than variables so that you can delete the variables file without deleting all of your precious warps.
This works for up to 54 warps.

Requirements: Skript-yaml

If you are looking for more skript-yaml help look at these two tutorials:
<https://github.com/Sashie/skript-yaml>
<https://skripthub.net/tutorials/25>

1) [Setting the warps](#Setwarp)
2) [Warp command](#Warp-command)
3) [Tab completions](#Tab-completions)
4) [Warp GUI](#Warp-GUI)
5) [Inventory click](#Inventory-click)
6) [Warp function](#Warp-function)
7) [Full code](#Full-code)

## Setwarp
We are going to create a command with 3 arguments. Name, Item, Description.
These arguments are going to be optional so that we can send more specific errors to the player.
And of course the player needs the permission to set the warp
```vb
command /setwarp [<text>] [<itemtype>] [<text>]:
	permission: skript.setwarp
	permission message: "&cYou are missing the required permission&4(&cskript.setwarp&4)"
	trigger:
		if arg-1 is set:
			if yaml value "warp.%arg-1%" from "warp" is not set: # making sure we do not set 2 warps under the same name
				if arg-2 is set:
					if arg-3 is set:
                        #CODE GOES HERE
					else:
						send "&3Please input a description of this warp"
				else:
					send "&3Please input an item to be displayed"
			else:
				send "&cA warp with that name already exists"
		else:
			send "&3Please input this warps name"
```
Now that we have the base of the command set out, all arguments are set and we arent setting 2 warps under the same name, lets set the warp in our YML file.
For this we need a YML file.
A good place to keep it is in your skript plugin folder.
I put my YML's under the `plugins/Skript/yamls/` path
Create a folder named `yamls` and create a file inside of that named `warp.yml`

To connect and change those yml values we need to load the yml file.
```vb
on load:
		load yaml "plugins/Skript/yamls/warp.yml" as "warp"
```
We can now reference that file with `from "warp"`
We can now replace `#CODE GOES HERE` with out setwarp code
```
set yaml value "warp.%arg-1%.item" from "warp" to "%arg-2%"
set yaml value "warp.%arg-1%.desc" from "warp" to arg-3
set yaml value "warp.%arg-1%.x" from "warp" to player's x coordinate
set yaml value "warp.%arg-1%.y" from "warp" to player's y coordinate
set yaml value "warp.%arg-1%.z" from "warp" to player's z coordinate
set yaml value "warp.%arg-1%.world" from "warp" to "%player's world%"
set yaml value "warp.%arg-1%.yaw" from "warp" to player's yaw
set yaml value "warp.%arg-1%.pitch" from "warp" to player's pitch
save yaml "warp" # saving the file so the data doesnt disappear 
send "&3Warp &e%arg-1% &3has been set to your location" # sending a message to the player to let them know it worked.
```
Our YML file will now look something like this:
```yml
warp:
    Spawn:
        item: nether star
        desc: Spawn for tutorial island
        x: -127.5
        y: 80.0
        z: -175.5
        world: world
        yaw: 89.899475
        pitch: -1.3759984
```
## Warp command
Now lets make the warp command(with arguments.)
```vb
command /warp [<text>]:
	cooldown: 5 seconds
	trigger:
		if arg-1 is set: # making sure that warp exists
			if yaml value "warp.%arg-1%" from "warp" is set:
				warp(player, arg-1) # this function does not exist yet but we will create that soon.
			else:
				send "&cThat warp does not exist"
        else:
            #GUI CODE GOES HERE
```

## Tab completions
You probably want to show all of the warps to the player while they are doing the on tab complete event.
```
on tab complete:
	if event-string is "/warp":
		set {_list::*} to yaml node list "warp" from "warp"
		loop {_list::*}:
			set {_x} to loop-value
			replace every "warp." in {_x} with ""
			add {_x} to {_list2::*}
		set tab completions for position 1 to {_list2::*}
```
## Warp GUI
As for the GUI code, we will use the minimum amount of rows needed for the warps.
Remember that the maximum amount of rows we can have is 6, meaning the maximum warps we can have is 54.
`set {_warps::*} to yaml node list "warp" from "warp"` will provide us with everything 1 under `warp` such as `warp.Spawn`
But we do not want the `warp.` part of that, we just want the `Spawn` so we will replace it in our skript.
```vb
			set {_warps::*} to yaml node list "warp" from "warp"
			loop {_warps::*}:
				set {_lv} to loop-value
				replace every "warp." in {_lv} with ""
				add {_lv} to {_warps2::*}
```
Now `{_warps2::*}` contains the name of all of our warps.
We can use this to set the slots in our GUI
To get the precist amount of rows we need we will use the `ceil` function (rounds up to the closest integer)
```vb
            set {_rows} to ceil(size of {_warps2::*}/9) # If we had 5 warps we would have 1 row, if we had 34 we would have 4 rows
			set {_gui} to a chest inventory with {_rows} rows named "&3&lWarps | &e&lClick to warp"
			loop {_warps2::*}: # loop-value = name of our warp
				set {_item} to yaml value "warp.%loop-value%.item" from "warp"
				set {_desc} to yaml value "warp.%loop-value%.desc" from "warp"
				set slot ({_x} ? 0) of {_gui} to ({_item} parsed as itemtype) named "&e&l%loop-value%" with lore "&b%{_desc}%"
				add 1 to {_x}
			set metadata value "warpgui" of player to {_gui}
			open {_gui} to player
```
Now we have the GUI opened to the player, lets make it work.
## Inventory click
All we need to do is check the inventory and use our function with the unformatted name of the event slot
```vb
on inventory click:
	if event-inventory = metadata value "warpgui" of player:
		cancel event
		warp(player, unformatted name of event-slot)
```
You can view our function below.
## Warp function
First off lets make the function with 2 arguments, player and text. This will be the player warping and the warp they are going to.
And lets get all the information we need. X, Y, Z, WORLD, YAW, PITCH
If you didnt know, yaw is the landscape direction the player is looking and pitch is the vertical direction the player is looking.
```vb
function warp(p: player, w: text):
	close {_p}'s inventory
	set {_x} to yaml value "warp.%{_w}%.x" from "warp"
	set {_y} to yaml value "warp.%{_w}%.y" from "warp"
	set {_z} to yaml value "warp.%{_w}%.z" from "warp"
	set {_world} to yaml value "warp.%{_w}%.world" from "warp"
	set {_yaw} to yaml value "warp.%{_w}%.yaw" from "warp"
	set {_pitch} to yaml value "warp.%{_w}%.pitch" from "warp"
	send "&eTeleportation started, please do not move" to {_p}
```
Now lets make the 5 second wait and the teleport part of our function.
For an explination on our this works check out [my simple spawn skript](https://github.com/Simpleton6969/Skripts/blob/main/Setspawn.md)
```vb
	set {_loc} to {_p}'s location
	send "5" to {_p}
	loop 5 times:
		wait 1 second
		if distance between {_loc} and {_p}'s location > 0.25:
			send "&cYou have moved, teleportation cancelled." to {_p}
			stop
		else:
			if loop-value < 5:
				send "%5-loop-value%" to {_p}
	teleport {_p} to location({_x}, {_y}, {_z}, world "%{_world}%", {_yaw}, {_pitch})
	send "&eYou have been teleported to &6%{_w}%" to {_p}
```
## Full code
Put all of that together and you get this:
```vb
on load:
		load yaml "plugins/Skript/yamls/warp.yml" as "warp"
command /setwarp [<text>] [<itemtype>] [<text>]:
	permission: skript.setwarp
	permission message: "&cYou are missing the required permission&4(&cskript.setwarp&4)"
	trigger:
		if arg-1 is set:
			if yaml value "warp.%arg-1%" from "warp" is not set:
				if arg-2 is set:
					if arg-3 is set:
						set yaml value "warp.%arg-1%.item" from "warp" to "%arg-2%"
						set yaml value "warp.%arg-1%.desc" from "warp" to arg-3
						set yaml value "warp.%arg-1%.x" from "warp" to player's x coordinate
						set yaml value "warp.%arg-1%.y" from "warp" to player's y coordinate
						set yaml value "warp.%arg-1%.z" from "warp" to player's z coordinate
						set yaml value "warp.%arg-1%.world" from "warp" to "%player's world%"
						set yaml value "warp.%arg-1%.yaw" from "warp" to player's yaw
						set yaml value "warp.%arg-1%.pitch" from "warp" to player's pitch
						save yaml "warp"
						send "&3Warp &e%arg-1% &3has been set to your location"
					else:
						send "&3Please input a description of this warp"
				else:
					send "&3Please input an item to be displayed"
			else:
				send "&cA warp with that name already exists"
		else:
			send "&3Please input this warps name"
command /warp [<text>]:
	cooldown: 5 seconds
	trigger:
		if arg-1 is set:
			if yaml value "warp.%arg-1%" from "warp" is set:
				warp(player, arg-1)
			else:
				send "&cThat warp does not exist"
		else:
			set {_warps::*} to yaml node list "warp" from "warp"
			loop {_warps::*}:
				set {_lv} to loop-value
				replace every "warp." in {_lv} with ""
				add {_lv} to {_warps2::*}
			set {_rows} to ceil(size of {_warps2::*}/9)
			set {_gui} to a chest inventory with {_rows} rows named "&3&lWarps | &e&lClick to warp"
			loop {_warps2::*}:
				set {_item} to yaml value "warp.%loop-value%.item" from "warp"
				set {_desc} to yaml value "warp.%loop-value%.desc" from "warp"
				set slot ({_x} ? 0) of {_gui} to ({_item} parsed as itemtype) named "&e&l%loop-value%" with lore "&b%{_desc}%"
				add 1 to {_x}
			set metadata value "warpgui" of player to {_gui}
			open {_gui} to player
function warp(p: player, w: text):
	close {_p}'s inventory
	set {_x} to yaml value "warp.%{_w}%.x" from "warp"
	set {_y} to yaml value "warp.%{_w}%.y" from "warp"
	set {_z} to yaml value "warp.%{_w}%.z" from "warp"
	set {_world} to yaml value "warp.%{_w}%.world" from "warp"
	set {_yaw} to yaml value "warp.%{_w}%.yaw" from "warp"
	set {_pitch} to yaml value "warp.%{_w}%.pitch" from "warp"
	send "&eTeleportation started, please do not move" to {_p}
	set {_loc} to {_p}'s location
	send "5" to {_p}
	loop 5 times:
		wait 1 second
		if distance between {_loc} and {_p}'s location > 0.25:
			send "&cYou have moved, teleportation cancelled." to {_p}
			stop
		else:
			if loop-value < 5:
				send "%5-loop-value%" to {_p}
	teleport {_p} to location({_x}, {_y}, {_z}, world "%{_world}%", {_yaw}, {_pitch})
	send "&eYou have been teleported to &6%{_w}%" to {_p}

on inventory click:
	if event-inventory = metadata value "warpgui" of player:
		cancel event
		warp(player, unformatted name of event-slot)
on tab complete:
	if event-string is "/warp":
		set {_list::*} to yaml node list "warp" from "warp"
		loop {_list::*}:
			set {_x} to loop-value
			replace every "warp." in {_x} with ""
			add {_x} to {_list2::*}
		set tab completions for position 1 to {_list2::*}```
