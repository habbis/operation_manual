# random shell stuff

Maybe tput is what you're looking for. For example you could create a little menu like so: 
```
!/bin/bash

# clear the screen
tput clear

# Move cursor to screen location X,Y (top left is 0,0)
tput cup 3 15

# Set a foreground colour using ANSI escape. Not sure this works on OSX
tput setaf 3

# Set reverse video mode
tput rev
echo "S U S H I  M E N U  I T E M S"
tput sgr0

tput cup 5 17
echo "Please select an option"
tput sgr0

tput cup 7 15
echo "1. Sushi Sashimi Combo"

tput cup 8 15
echo "2. Omakase"

tput cup 9 15
echo "3. Chirashi"

tput cup 10 15
echo "4. Eel and Avacado Roll"


# Set bold mode 
tput bold
tput cup 12 15
read -p "Enter your choice [1-4] " choice

tput clear
tput sgr0
tput rc

echo "You chose $choice"
```
