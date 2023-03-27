# Variables
Your variables are wrong! That is why you are here.    
No worries! We can fix that quickly.
1) [Local variables](#local-variables)
2) [Global variables](#global-variables)
3) [List variables](#list-variables)
4) [RAM variables](#ram-variables)
5) [Option](#options)
6) [Tips and tricks](#tips-and-tricks)
7) [Metadata](#Metadata)

There are essential things to consider when setting a variable.    
Such as what type of variable to use. So let's discuss the different variable types and how to use them.

## Local Variables
Local variables will only be stored inside that trigger, meaning that you can only use them in that segment of the script.   
They are suitable for quickly storing data you will not need elsewhere.

The code below is a example of WRONGLY using local variables.
```vb
command /setspawn:
    trigger:
        set {_spawn} to player's location # <==========
        send "Spawn has been set to your location. &e/spawn"
command /spawn:
    trigger:
        teleport player to {_spawn}
```
This is incorrect because when the player tries to use their spawn command, they will realize that the variable is not set.   

The correct use of a local variable can be seen below:
```vb
command /healthcheck:
    trigger:
        set {_health} to player's health
        wait 1 second
        if {_health} = player's health:
            send "Your health has remained the same!"
```
This is correct because it is not storing the data permanently when it doesn't need to.

## Global Variables
To fix the setspawn script we need to use a global variable.   
Global variables are variables that are stored permanently, we will be talking about this next.   
Some objects in Skript cannot be stored in variables permanently, such as entities, this is due to the fact they aren't serialized.   
If you attempt this, you will get a warning. The variable can be stored until the server stops.

The fixed script is below.
```vb
command /setspawn:
    trigger:
        set {spawn} to player's location # <==========
        send "Spawn has been set to your location. &e/spawn"
command /spawn:
    trigger:
        teleport player to {spawn}
```

But you could also be using global variables wrong.   
As seen below:
```vb
command /healthcheck:
    trigger:
        set {health} to player's health
        wait 1 second
        if {health} = player's health:
            send "Your health has remained the same!"
```
This isn't good because anybody could run that command during the wait, and the health variable will be set to their health and not yours.    
This would be a good time to use a local variable, since it doesn't need to be stored outside of this trigger.   
Another option is using a list variable. We'll talk about that next.

## List Variables
The final type of variable is a list variables.   
List variables are.... well... they're lists :D
We can use them to bulk store data that can be looped or is easily accessible.

Below is a common misconception of what list variables are; this is not a list variable and is bad :(
```vb
command /setme:
    trigger:
        set {setme.%player%} to true
```
This is bad is because we cannot loop everything inside of the setme variable.   
Below is an example of a list variable
```vb
command /setme:
    trigger:
        set {setme::%player's uuid%} to true
```
Now we can loop that variable and do stuff with it.   
To access everything inside of a list variable, put `*` after `::`
```vb
command /loopme:
    trigger:
        loop {setme::*}:
            if loop-value = true:
                broadcast "True yay"
```
An example of a good usage of loop variables is below.   
It will track all the attackers of a victim
```vb
on damage:
    add attacker's name to {attackers::%victim's uuid%::*} 
on death:
    broadcast "All attackers of %victim%: %{attackers::%victim's uuid%::*}%"
    delete {attackers::%victim's uuid%::*}
```

## RAM Variables
Another type of variable you could use is called a ram variable.   
They are used for storing data until the server stops.   
You can use these to objects which do not need to persist thru server restarts.

``` vb
command /spawnbob:
  trigger:
    spawn a creeper at player's location
    set {-creeper} to last spawned entity
```
These are useful for some people's uses.

### Enabling RAM Variables
Unfortunately, ram vars aren't enabled by default, and you must enable them in the skript config.   
Simply go into the skript config file and find the databases section near the bottom.    
Next, you will need to find and replace the following:
```
		type: CSV
		
		pattern: .*
		
		file: ./plugins/Skript/variables.csv
		
		backup interval: 2 hours
```
With:
```
		type: CSV
		
		pattern: (?!-).*
		
		file: ./plugins/Skript/variables.csv
		
		backup interval: 2 hours
```
You can test if you have ram vars enabled by using this ssript below.   
Simply set the variable, restart your server and test the variable.   
If your name is sent in chat, then ram vars aren't enabled, if 0 is, ram vars are enabled.
```vb
command /sets:
    trigger:
        set {-ram} to player's display name
        broadcast "{-ram} has been set to %{-ram}%"
command /test:
    trigger:
        broadcast ({-ram} ? 0)
```

## Options
Options are fantastic for configurability; for example, if you plan on handing your script to somebody else or changing the value of something in your editor quickly, then options are what you're looking for.   
You can use an option like this:
```vb
options:
    prefix: &2&lMyServerName
    price: &6$400

command /ServerPrice:
    trigger:
        send "You can buy {@prefix} for {@price}" to player
```

## Tips and Tricks
You may have also been sent here because you are using player's name in variables.   
This is bad because if the player changes their name, they will lose all of their precious data and grinding time you have made for them.    
To fix this, all we need to do is replace our variables with better variables :D.   
Instead of `{var.%player%}` use `{var::%player's uuid%}`

## Metadata
Metadata is another form of data storage that can be *attached* to players.
Metadata tags are deleted once a player leaves or server restarts.
It can be accessed and set the same way variables can

Some examples below:
```
set (metadata value "tagname" of player) to 10 seconds after now
if (metadata tag "tagname" of attacker) > now:
```
Metadata is a great storage method for inventories and combat logging.

It is important to note with metadata that putting the tags in () when setting or checking them will greatly decrease your reload speed.
