Loops are magical tools used for a wide range of things in skript. You can loop lots of things, a number, a variable, players entities and more!
They are a vanilla tool that we will discuss in this tutorial.

1) [Loop x times](#Loop-x-times)
2) [Looping a variable](#Looping-a-variable)
3) [What is a loop-value?](#What-is-a-loop-value?)
4) [What is a loop-index?](#What-is-a-loop-index?)
5) [Looping blocks](#Looping-blocks)
6) [Looping players/entities](#Looping-players/entities)
7) [Loop filters](#Loop-filters)
8) [While loops](#While-loops)

## Loop x times
You can loop an integer of times in skript, for example `loop 10 times:` or `loop {_int} times:`
You can do this to make a countdown.
```vb
command /countdown <integer>:
	trigger:
		loop arg-1 times:
			broadcast (arg-1)-(loop-value)
```
## Looping a variable
[List variables](https://github.com/Simpleton6969/Skripts/blob/main/Variables.md#list-variables) can be looped, meaning the values can be checked and so can their indexes.
One use of looping a variable is setting slots in a GUI, check out my [warpGUI](https://github.com/Simpleton6969/Skripts/blob/main/WarpGUI.md) tutorial for an example.

## What is a loop-value?
While looping a variable a loop-value is the value the variable is set to. (eg: `{list::var} = 12` while looping that list, the value of the loop variable will be 12)
While looping an amount of times, the loop-value is the number of the loop. As shown above.

## What is a loop-index?
A loop-index is the name of the variable being looped.
For example, if we have these variables:
```yml
{list::banana} = 12
{list::apple} = 15
{list::potato} = 17
{list::grapes} = 12
{list::pears} = 5
```
Then the loop indexes will be: `banana`, `apple`, `potato`, `grapes`, `pears`
And the loop values will be: `12`, `15`, `17`, `12`, `5`
## Looping blocks
You can use loops to get the blocks between two locations (a line), an example of this can be seen below.
```vb
command /bridge:
  trigger:
    loop blocks between (players location) and (location 10 blocks infront of players location):
      set loop-block to diamond block
      wait 5 seconds
      set loop-block to air
```
You can also loop the blocks within two locations to make a cuboid, as seen below.
```vb
command /bridge:
	trigger:
		loop blocks within (player's location) and (location 10 blocks infront of player):
			set block at location of loop-block to diamond block
```

## Looping players/entities
Similarly to looping blocks, you can also loop players and entities. 
A good utility of this is in periodical events, where normally there arent players. But by looping all players you can use loop-player(loop-entity for entities).
```vb
every second:
  loop all players:
    push loop-player upwards with force 3
command /entityfly:
  loop all players:
    push loop-entity upwards with force 3
```
## Loop filters
Instead of using another line for an if statement, there are many things you can filter within the loop line. Some examples are below.
`loop all players where [input != player]:`
`loop all blocks within {_loc1} and {_loc2} where [input = air]:`
`loop all entities where [name of input = "Bob"]:`
## While loops
While loops can be used as an if statement ran repeatedly.
You can use these for many things such as while player is online, while server is online, while num1 > num2.
It is important to note that while loops without a proper exit will loop infinitely and will crash your server.
For example, while player is online will crash our server.
```vb
on join:
  while player is online:
    give player 1 of wooden sword
    # We need to add a wait
    wait 1 second
command /morethan50:
  trigger:
    set {_x} to 1
    set {_y} to 5
    while {_x} < {_y}:
      add 1 to {_x}
      broadcast "x is greater than y"
```
