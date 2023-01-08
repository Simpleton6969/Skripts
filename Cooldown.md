Cooldown skript you say? Look no further!
No requirements and easy to change for your situation.

To get started we will create a function for adding to the cooldown of the player.
For this I am using metadata because my cooldowns are not a long time.
Metadata is deleted when a player leaves the server.
```
function cooldownAdd(p: player, i: integer): # this requires a player and an integer of seconds to add.
    set {_t} to ("%{_i}% seconds" parsed as time span) after now # we are turning that integer into that amount of seconds after now
    set metadata value "cooldown" of {_p} to {_t} # now we are setting the cooldown to something on the player
```
If you wish to replace metadata with a variable and use minutes (you can change to hours or other timespans)
Then use the code below.
```
function cooldownAdd(p: player, i: integer):
    set {_t} to ("%{_i}% minutes" parsed as time span)
    set {cooldown::%{_p}'s uuid%} to {_t}
```

Now that we have the adding to the cooldown sorted, we need a way to check the cooldown.
We are going to need a function that returns something to us.
We are simply checking if the stored data is after now, if it is the cooldown is active and the function will return true
If it isnt then the function will return false and the cooldown will not be active.
```
function cooldownCheck(p: player) :: boolean:
    set {_t} to ((metadata value "cooldown" of {_p}) ? now)
    if {_t} > now:
        return true
    return false
```

If you are using variables and not metadata, then replace the second line with this:
```
    set {_t} to ({cooldown::%{_p}'s uuid%} ? now) 
```
The ( ? now) makes sure than if the player has never had a cooldown set then it will display false and they will not be on cooldown.
And theres our functionality made.
But you probably want to call these functions and use them in your server.
To do this we can use the code below.
```
command /every10seconds:
  trigger:
    if cooldownCheck(player) = true:
      # player is on cooldown
    else:
      # player is off cooldown
      cooldownAdd(player, 10)
```

Thats all really, a singular use cooldown.
Full code without comments:
```
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
            cooldownAdd(player, 10)
```

This is a really cool function to be adapted for several uses, such as below

```
function cooldownAdd(p: player, i: integer, cool: text):
    set {_t} to ("%{_i}% minutes" parsed as time span)
    set {player::%{_cool}%::%{_p}'s uuid%} to {_t}

function cooldownCheck(p: player, cool: text) :: boolean:
    set {_t} to ({player::%{_cool}%::%{_p}'s uuid%}) ? now)
    if {_t} > now:
        return true
    return false

command /every10seconds <text>:
  trigger:
    if cooldownCheck(player, text) = true:
      # player is on cooldown
    else:
      # player is off cooldown
      cooldownAdd(player, 10, arg-1)
```
