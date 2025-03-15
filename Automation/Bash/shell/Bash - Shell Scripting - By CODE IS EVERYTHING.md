# Become a shell wizard in ~12 mins

https://youtu.be/IYZDIhfAUM0?si=AwgdTJB-BNP7BgYC
Description:
we're running through all the important things you need to know in order to get comfortable using the shell and see how you can compose commands together to build out super handy chains that'll save you a lot of time.

#### Shell Wizardry
###### The Basics
Name: $SHELL, tty, console, command line.

Terms to get relevant

+ commands - cmd - the program to do some actionable tasks (like ls, pws, cd)
+ flags - one that comes with like sub command to run how the cmd has to execute, -l as list, -a for all, -t - time, -r - in reverse/recursive, -h human readable file sizes
+ cmd line syntax comibination -> cmd -flag file/dir
+ echo 'cmd' - lets your print some text
+ cat - prints the contents in the file
+ touch - create a file
+ cp - copies file or dir, to the target destination //cp target destination
+ mv - moves file or dir, to the target destination //mv target destination
+ rm - removes file. with flag -r - recursive, -f - force
+ ln - creates hard link, -s - creates symlink 'file' 'otherfileDiffName'
+ less - prints contents in a scrollable format (useful, easy to read, search, /search, :scroll )
+ more - similar to less but less adapted to the screen size
+ cmd --help - shows docs of a cmd or its flags or params
+ man - manual or documentation of a program
+ grep 'word/content' - pattern matches the word or content in a file
+ rg 'word/content' - ripgrep - grep but faster
+ find/fd -name 'node_modules' -type d - find files in a directory
+ sed =stream editor which helps you make changes on an incoming stream of text
--> cat template.yml
	paul the duke
	email: REPLACE_EMAIL
	url: https://atreidies.com
--> SYNTAX: sed 's/RemovalWord/AdditionalWord' filename
--> sed 's/REPLACE_EMAIL/'paul@atreides.com/' template.yml
	paul the duke
	email: paul@atreides.com
	url: https://atreidies.com
+ 


---
#### Become a bash scripting pro - full course

https://youtu.be/4ygaA_y1wvQ?si=zvaLoDj6UU3BAHbI
learn everything you need to know in order to start writing bash scripts to automate all of your boring tasks. Useful reference site: [https://learnxinyminutes.com/docs/bash/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbENXOGFqQVpQbk5pNTdiZnJPbEZNSmFwcE1Ed3xBQ3Jtc0ttRWgwd0F2Tk53SDJiNUllZGdpYUhGT2FmVHljc1F3bDBTTFcxNGFydm5FVEVBU04yd1JjTk4yRHdGRUk5SGt1N2lyd3RMQWJiZmRNZllIX1NBRmZRU0tZRWY0SG5heVJ6MWdxS2l5bnBRUUZKdDhKaw&q=https%3A%2F%2Flearnxinyminutes.com%2Fdocs%2Fbash%2F&v=4ygaA_y1wvQ)


