Functions! Theyre an amazing thing that will stop you from copying and pasting the same code 500 times.
In this tutorial we wll go over what functions are, what types of functions there are, and when you should use them.

1) [What is a function?](#What-is-a-function?)
2) [Making our own function](#Making-our-own-function)
3) [Parameters](#Parameters)
4) [Calling a function](#Calling-a-function)
5) [Returning a value](#Returning-a-value)

## What is a function?
Functions are segments of code that you can make skript run without copying and pasting that code.
They are basically commands but for skript.
You can make them with or without arguments and with or without a return value

There are a lot of useful functions that already exist in skript, such as the location function.
You can find a full list of functions [on the skripthub docs](https://skripthub.net/docs/).

## Making our own function
To create a function you only really require one thing, a name. 
You cannot use the same name for several functions and cannot use special characters such as spaces, ?, -, ., /, !.
You can however use underscores _ in function names.

The format for creating a function is below.
```vb
function functionName():
  #code
  ```
## Parameters
A paramater is an argument for your function.
You can view a [full list of Parameters and a brief description here](https://dev.bukkit.org/projects/skript/pages/custom-commands#title-3)
  
Because a function is not something a player can call there are no players inside of a function event. 
So we will have to use a player Parameters.
As shown below.
```vb
function functionName(parameterName: parameterType):
# So lets create our function
function clearChatPlayer(p: player):
  loop 500 times:
    send " " to {_p}
```
## Calling a function
To call our function we just have to use its name in our code and input any paramaters if needed.
Lets create a command for moderators to use to clear the chat of an online player.
And lets make it so if an argument is not given then it will be the player executing the command by default.
```vb
command /clearchat [<player=%player%>]:
  permission: skript.chat.clear
  trigger:
    clearChatPlayer(arg-1)
```
And that summarises basic functions...... but why dont we make the function return something to us rather than execute elsewhere.

## Returning a value
There are many things we can return to the skript, such as: numbers, integers, strings, booleans(true/false), object and so on.
It is important to note that returning a value instantly exits the function.

Lets use that to create a system for basic cooldowns. 
For a more in detail tutorial check [my cooldown tutorial](https://github.com/Simpleton6969/Skripts/blob/main/Cooldown.md)
```vb
function cooldownCheck(p: player) :: boolean:
    set {_t} to ((metadata value "cooldown" of {_p}) ? now)
    if {_t} > now:
        return true
    return false
function cooldownAdd(p: player, i: integer):
    set {_t} to ("%{_i}% seconds" parsed as time span) after now
    set metadata value "cooldown" of {_p} to {_t}
command /10secondcommand:
    trigger:
        if cooldownCheck(player) = true:
            #player is on cooldown
            send "Please wait"
        else:
            #player is off cooldown
            send "You are off cooldown"
            cooldownAdd(player, 10)
```
