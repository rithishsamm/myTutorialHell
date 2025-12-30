```BASH
#!/bin/bash

#this script is about variable, a value/datatype that holds within a string, namely VAR.

#you can compose or call in in a script as per your convenience or rElevant to the logic applies
#intro :

MESSAGE="Hello World"
NAME="RITHISH SAM"
#no spaces should exist around "="

#if = -> takes as VARIABLE ASSIGNMENT
#if = with spaces, VAR isnt a VAR but command and tries to execute it, which doesnt work

# If you think about it, this makes sense.
#how else could you tell it to run the command VAR with its first argument being "="
#and its second argument being "value"?

echo "$MESSAGE", Myself "$NAME"
#when calling a VAR, " " -> must thing, !important.
```

