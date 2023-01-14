Need a scoreboard skript? No problem. 
This tutorial will teach you how to make a scoreboard using skbee that will update every second.
Remember, when testing your scoreboard, you must leave and rejoin to see any changes made.

Requirements: SkBee

1) [Scoreboard function](#Scoreboard-function)
2) [Calling the function](#Calling-the-function)
3) [Toggling scoreboard](#Toggling-scoreboard)
4) [Full code](#Full-code)

## Scoreboard function
To get started we will create a function for the setting of the scoreboard. 
This basic one will have a title and two lines which you can add to.
Anything where you would normally use %player% can be replaced with %{_p}%
```vb
function scoreboard(p: player): # Creating a function and inputting a player to provide the scoreboard to
    set title of {_p}'s scoreboard to formatted "&3Title" # sets the title of the scoreboard
    set line 2 of {_p}'s scoreboard to "&2%{_p}%" # sets line 2 of the scoreboard
    set line 1 of {_p}'s scoreboard to "&aLine 1" # sets line 2 of the scoreboard
```

## Calling the function
Next we need to call the function to provide the player with the scoreboard.
For this we are going to utilise the `on join` event and make a while loop for the player being online. 
This is the least laggy method possible.
```vb
on join:
    while player is online:
        scoreboard(player) # calls the scoreboard function
        wait 1 second # every while loop needs a wait otherwise the server would crash.
```
## Toggling scoreboard
Finally, you can give the player the ability to toggle this scoreboard.
They can use the command or any of the aliases to enable or disable their scoreboard
```vb
command /toggle:
    aliases: /togglesc , /togglescoreboard , /scoreboard
    trigger:
        toggle player's scoreboard
```

## Full code
```vb
function scoreboard(p: player):
    set title of {_p}'s scoreboard to formatted "    &8&lUnmanned    "
    set line 2 of {_p}'s scoreboard to "&2%{_p}%" # sets line 2 of the scoreboard
    set line 1 of {_p}'s scoreboard to "Line1"

on join:
    while player is online:
        scoreboard(player)
        wait 10 ticks
command /toggle:
    aliases: /togglesc , /togglescoreboard , /scoreboard
    trigger:
        toggle player's scoreboard
```
Hope this is useful for you, thanks for your time.
