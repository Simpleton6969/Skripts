The beginners guide to good indentation.
Indentation is one of the most important parts of coding, without it, your code would simply not work.

Let's get started by summarising what indentation is, indentation is when you bring a line of code forward one indent.
In skript that indentation can be many things, tabs and spaces. You can indent with any amount of tabs and spaces that you want.
It is recommended though that if you use tabs, use 1 tab. And if you use spaces, use 4 spaces for each indent.

When choosing what to indent, it is important to remember how you indented. 
The reason for this is you cannot switch between spaces and tabs for indentation and you cannot switch how much you indent (aka from 2 to 4 to 2 to 6 etc).

A very simple way of knowing how to indent is after every: in your code. Some exceptions to this apply, and we will cover those too.
Another thing many people suffer with is mixing up their indentation methods. You cannot mix spaces and tabs in your skript. If you use one method, stick to that one method.

Here is an example of a WRONGLY indented skript
```vb
command /command <text>:
  trigger:
    if arg-1 is "Text":
    send "%arg-1%"
      else:
          send "Text needs to be sent"
```
We can improve this by indenting correctly.
```vb
command /command <text>:
  trigger:
    if arg-1 is "Text":
      send "%arg-1%"
    else:
      send "Text needs to be sent"
```
One thing I see many people mess up on when creating if statements is forgetting the colon at the end of the line and not aligning the else statement with the if statement.
Every else statement needs an if statement, and to connect the right else with the right if they need to be on the same indentation level.

Down below is an example of those exceptions we were talking about earlier, using variables and options.
```vb
variables:
  {Bananas} = 1
options:
  prefix: &2&lMyServerName
command /serverbananas:
  trigger:
    if {bananas} = 1:
      send "You are the first to run this command on {@prefix}"
      add 1 to {bananas}
    else if {bananas} = 2:
      send "You are the second to run this command on {@prefix}"
      add 1 to {bananas}
    else:
      send "This command has been ran %{bananas}% times on {@prefix}"
      add 1 to {bananas}
```

Another example of WRONGLY indented code is below
```vb
on right click:
  if name of player's tool is "&5Simpletons sword"
  push player upwards with force 3
```
That will throw an error for that line because it does not end with a colon, then it will throw another error because the third line is not indented.
It will push everybody who right clicks upwards.

Check below for improved code
```vb
on right click:
  if name of player's tool is "&5Simpletons sword":
    push player upwards with force 3
```

If statements arent the only thing requiring indentation though, loops, functions, commands, triggers and more also require indentation.
```vb
command /PowerOF <integer> <number>:
  trigger:
    set {_po} to PowerOf(arg-1, arg-2)
    send "%arg-1% to the power of %arg-1% is %{_po}%"
function PowerOf(t: integer, i: number):: number:
  set {_i} to {_i}^{_t}
  return {_i}
```
