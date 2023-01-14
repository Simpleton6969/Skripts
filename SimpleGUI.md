Struggling with making GUI's with vanilla skript? I can help!
No requirements

1) [Command to create GUI](#GUI-command)
2) [Code to create GUI](#GUI-code)
3) [Inventory click](#Inventory-code)
4) [Full code](#Full-code)

For this we are gonna use a command with an integer argument the player can use to make a custom amount of rows.
## GUI command
```vb
command /simplegui [<integer>]: # creates a command with an optional argument so we can send custom messages
	trigger:
		if arg-1 is set: # making sure the argument is set
			if arg-1 >= 1: # making sure the argument is above or equal to 1, you cannot have a gui with 0 rows
				if arg-1 <= 6: # making sure the argument is below or equal to 6, you cannot go above 6
					# GUI code
				else: # else conditions must allign with their if statement
					send "A chest GUI must have 1-6 rows"
			else:
				send "A chest GUI must have 1-6 rows"
		else:
			send "Specify an integer from 1-6"
```
Now we have the base of the command made so that we can make a GUI; let's make the GUI!
## GUI code
Theres many different ways to set slots in a GUI, here are some examples.
By the way, the "first slot" of an inventory is slot 0!
```vb
set slot 11 of {_gui} to 12 glass
set slots 12,15,17 of {_gui} to  63 dirt
set slots (integers from 0 to 8) to player's skull named "%player%"
```
We are going to set the top row to red stained glass panes and the middle slot of it to the player's skull
```vb
set {_gui} to a chest inventory with arg-1 rows named "%player%'s skull" # creates a gui with a name and sets it to a local variable
set slots (integers from 0 to 8) of {_gui} to red stained glass pane named "&4Red" # sets the first row of the gui to red stained glass pane
set slots (integers from 9 to (arg-1)*9-1) of {_gui} to black stained glass pane named "&0" # sets all other rows to black stained glass pane
set slot 4 of {_gui} to player's skull named "%player%" # sets slot 4 to the players skull named after the player
set metadata value "gui" of player to {_gui} # setting the metadata of the player makes it easy to check inventories and code them further down the line
open {_gui} to player # lastly with a gui, we need to open it to a player
```
Put them together and you have a GUI skript!

Why don't we make it do things when we click slots in the inventory, though :D
For this, we can use the on inventory click event. There are several different methods of checking slots, such as index (slot number), name of slot and type of slot.
## Inventory click
```vb
on inventory click:
	if event-inventory = metadata value "gui" of player: # checking the inventory is the inventory we made by comparing it with the metadata of the player
		cancel event # making it so we cannot move the inventory slots around
		if index of event-slot = 4: # checking index of slot to index of players skull
			push player up with force 3
			give player 1 of player's skull
		else if event-slot is black stained glass pane: # checking if the clicked slot is a black stained glass pane
			close player's inventory
		else if name of event-slot is "&4Red": # checking if the name of the event slot is "&4Red"
			play sound "entity.experience_orb.pickup" to player
```
## Full code
```vb
command /simplegui [<integer>]:
	trigger:
		if arg-1 is set:
			if arg-1 >= 1:
				if arg-1 <= 6:
					set {_gui} to a chest inventory with arg-1 rows named "%player%'s skull"
					set slots (integers from 0 to 8) of {_gui} to red stained glass pane named "&4Red"
					set slots (integers from 9 to (arg-1)*9-1) of {_gui} to black stained glass pane named "&0"
					set slot 4 of {_gui} to player's skull named "%player%"
					set metadata value "gui" of player to {_gui}
					open {_gui} to player
				else:
					send "A chest GUI must have 1-6 rows"
			else:
				send "A chest GUI must have 1-6 rows"
		else:
			send "Specify an integer from 1-6"

on inventory click:
	if event-inventory = metadata value "gui" of player:
		cancel event
		if index of event-slot = 4:
			push player up with force 3
			give player 1 of player's skull
		else if event-item is black stained glass pane:
			close player's inventory
		else if name of event-slot is "&4Red":
			play sound "entity.experience_orb.pickup" to player```
