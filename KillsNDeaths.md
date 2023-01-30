In this skript we will be making a combat tracker skript with kills, deaths, KD, and kills this game.
We will be able to check these stats in our scoreboard and also create a command to check the information of others.

1) [Variable setting](#Variable-setting)
2) [KDA and kills this game](#KD-and-stats-this-game)
3) [Info checking](#Info-checking)
4) [Scoreboard](#Scoreboard)
5) [Full code](#Full-code)

## Variable setting
To get started we will need to create the variable setting.
We will use the `on death of player` event for this.
It is also wise to keep all of the pvp stats under a `{pvp::*}` list so we can clear it whenever we want.
```vb
on death of player:
  add 1 to {pvp::%victim's uuid%::deaths}
  if attacker is a player:
    add 1 to {pvp::%attacker's uuid%::kills}
```
This will add 1 to the deaths of the player whenver they die from anything, and 1 to the kills of the attacker when they kill a player
## KD and stats this game
Lets start by setting a Ram variable for the kills this game.
Check out my variables tutorial [here](https://github.com/Simpleton6969/Skripts/blob/main/Variables.md) on how to enable and use Ram variables.
```vb
on death of player:
  add 1 to {pvp::%victim's uuid%::deaths}
  add 1 to {-DTG::%attacker's uuid%}
  if attacker is a player:
    add 1 to {pvp::%attacker's uuid%::kills}
    add 1 to {-KTG::%attacker's uuid%}
```
Now we need to delete that variable when the player leaves or joins, we are going to use both because if the server crashes we cannot use the on quit event.
```vb
on join:
  delete {-KTG::%player's uuid%}
  delete {-DTG::%player's uuid%}
on quit:
  delete {-KTG::%player's uuid%}
  delete {-DTG::%player's uuid%}
```
KD is something that can be easily be calculated by dividing kills by deaths, if deaths is not set then we need to use a default value of 0.
We can use that by doing `"KD: %{pvp::%player's uuid%::kills}/({pvp::%player's uuid%::deaths} ? 0)%"`

## Info checking
We will create a command to check either yours or another players pvp stats. 
We will use an optional argument of offline player (default player if argument is not made) so we can get stats of offline players as well as online players and if an argument is not set it will display your stats.
```vb
command /pvpstats [<offline player=%player%>]:
  trigger:
    if arg-1 is set:
      send "&c&lPVP stats of %arg-1%"
      send "&6Overall:"
      send "&e>> Kills: &7%{pvp::%arg-1's uuid%::kills} ? 0%"
      send "&e>> Deaths: &7%{pvp::%arg-1's uuid%::deaths} ? 0%"
      send "&e>> KD: &7%({pvp::%arg-1's uuid%::kills} ? 0)/({pvp::%arg-1's uuid%::deaths} ? 0)%"
      send "&6This game:"
      send "&e>> Kills: &7%{-KTG::%arg-1's uuid%} ? 0%"
      send "&e>> Deaths: &7%{-DTG::%arg-1's uuid%} ? 0%"
      send "&e>> KD: &7%({-KTG::%arg-1's uuid%} ? 0)/({-DTG::%arg-1's uuid%} ? 0)%"
```
## Scoreboard
Please look at my scoreboard tutorial [here](https://github.com/Simpleton6969/Skripts/blob/main/Scoreboard.md) for more information on this 
All we are going to do is add our variables onto the scoreboard we already have.
```vb
function scoreboard(p: player):
    set title of {_p}'s scoreboard to formatted "    &c&lPVP stats    "
    set line 8 of {_p}'s scoreboard to "&6&lOverall:"
    set line 7 of {_p}'s scoreboard to "&eKills: &7%{pvp::%{_p}'s uuid%::kills} ? 0%"
    set line 6 of {_p}'s scoreboard to "&eDeaths: &7%{pvp::%{_p}'s uuid%::deaths} ? 0%"
    set line 5 of {_p}'s scoreboard to "&eKD: %({pvp::%{_p}'s uuid%::kills} ? 0)/({pvp::%{_p}'s uuid%::deaths} ? 0)%"
    set line 4 of {_p}'s scoreboard to "&6&lThis game:"
    set line 3 of {_p}'s scoreboard to "&eKills: &7%{-KTG::%{_p}'s uuid%} ? 0%"
    set line 2 of {_p}'s scoreboard to "&eDeaths: &7%{-DTG::%{_p}'s uuid%} ? 0%"
    set line 1 of {_p}'s scoreboard to "&eKD: &7%({-KTG::%{_p}'s uuid%} ? 0)/({-DTG::%{_p}'s uuid%} ? 0)%"

on join:
    while player is online:
        scoreboard(player)
        wait 10 ticks
command /toggle:
    aliases: /togglesc , /togglescoreboard , /scoreboard
    trigger:
        toggle player's scoreboard
```
## Full code
```vb
on death of player:
  add 1 to {pvp::%victim's uuid%::deaths}
  add 1 to {-DTG::%attacker's uuid%}
  if attacker is a player:
    add 1 to {pvp::%attacker's uuid%::kills}
    add 1 to {-KTG::%attacker's uuid%}
on join:
  delete {-KTG::%player's uuid%}
  delete {-DTG::%player's uuid%}
on quit:
  delete {-KTG::%player's uuid%}
  delete {-DTG::%player's uuid%}
command /pvpstats [<offline player=%player%>]:
  trigger:
    send "&c&lPVP stats of %arg-1%"
    send "&6Overall:"
    send "&e>> Kills: &7%{pvp::%arg-1's uuid%::kills} ? 0%"
    send "&e>> Deaths: &7%{pvp::%arg-1's uuid%::deaths} ? 0%"
    send "&e>> KD: &7%({pvp::%arg-1's uuid%::kills} ? 0)/({pvp::%arg-1's uuid%::deaths} ? 0)%"
    send "&6This game:"
    send "&e>> Kills: &7%{-KTG::%arg-1's uuid%} ? 0%"
    send "&e>> Deaths: &7%{-DTG::%arg-1's uuid%} ? 0%"
    send "&e>> KD: &7%({-KTG::%arg-1's uuid%} ? 0)/({-DTG::%arg-1's uuid%} ? 0)%"
```
