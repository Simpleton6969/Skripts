Your variables are bad! That is why you are here isnt it?
No worries! We can fix that easily.
```
1) Local variables
2) Gloabl variables
3) Ram variables
4) List variables
5) Option variables
6) Tips and tricks
```
Theres important things to consider when setting a variable.
Such as what type of variable to use. So lets disuss the different types of variables and how to use them.

Local variables are variables that will only be stored inside that trigger, meaning that you can only use them in that segment of skript.
They are good for quickly storing data you will not need elsewhere.

In the code below is a bad example of how to use local variables.
```
command /setspawn:
	trigger:
		set {_spawn} to player's location # <==========
		send "Spawn has been set to your location. &e/spawn"
command /spawn:
  trigger:
    teleport player to {_spawn}
```

The reason this is bad is because when the player tries to use their spawn command they will realise that the variable is not set.
A good use of a local variable can be shown below:
```
command /healthcheck:
  trigger:
    set {_health} to player's health
    wait 1 second
    if {_health} = player's health:
      send "Your health has remained the same!"
```
This is good because it is not storing the data permanently when it doesnt need to.

To fix the setspawn skript we need to use a global variable
Global variables are variables that are stored permanently.
Unfortunately you cannot store mobs in variables permanently, you can store them in a global var until the server stops.

The fixed skript is below.
```
command /setspawn:
	trigger:
		set {spawn} to player's location # <==========
		send "Spawn has been set to your location. &e/spawn"
command /spawn:
  trigger:
    teleport player to {spawn}
```

But you could also be using global variables wrong.
As seen below.
```
command /healthcheck:
  trigger:
    set {health} to player's health
    wait 1 second
    if {health} = player's health:
      send "Your health has remained the same!"
```
The reason this is bad is because anybody could run that command during the wait and the health variable will be set to their health and not yours.

Another type of variable you could use is called a ram variable.
They are used for storing data until the server stops
You can use these to store mobs so you do not get a caution error in your skript.

``` 
command /spawnbob:
  trigger:
    spawn a creeper at player's location
    set {-creeper} to last spawned entity
```
These are useful for some peoples uses.
Unfortunately ram vars arent enabled by default and you have to enable them yourself in the skript config.
Simply go into the skript config file and find the databases section located near the bottom of the file.
Next you will need to find and replace:
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

The final type of variables are list variables.
List variables are.... well... they're lists :D
We can use them to bulk store data that can be looped or is easily accessable.

Below is a common misconception of what list variables are, this is not a list variable and is bad :{
```
command /setme:
  trigger:
    set {setme.%player%} to true
```
The reason this is bad is because we cannot loop everything inside of the setme variable.
Below is an example of a list variable
```
command /setme:
  trigger:
    set {setme::%player's uuid%} to true
```
Now we can loop that variable and do stuff with it.
To access everything inside of a list variable, we can put * after ::
```
command /loopme:
  trigger:
    loop {setme::*}:
      if loop-value = true:
        broadcast "True yay"
```
An example of a good usage of loop variables is below.
It will track all the attackers of a victim
```
on damage:
  add attacker's name to {attackers::%victim's uuid%::*} 
on death:
  broadcast "All attackers of %victim%: %{attackers::%victim's uuid%::*}%"
  delete {attackers::%victim's uuid%::*}
```
Option variables are a great thing for configurability, for example, if you plan on handing your skript to somebody else or changing the value of something in your editor easily then option variables are what you're looking for.
You can use an option variable like this:
```
options:
	prefix: &2&lMyServerName
	price: &6$400

command /ServerPrice:
	trigger:
		send "You can buy {@prefix} for {@price}" to player
```

You may have also been sent here because you are using player name in variables.
The reason this is bad is because if the player changes their name they will lose all of their precious data and grinding time you have made for them.
To fix this all we need to do is replace our variables with better variables :D
Instead of `{var.%player%}` use `{var::%player's uuid%}`
