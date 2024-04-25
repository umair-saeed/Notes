# _Bash Scripting Reference_

This document contains the reference commands for Bash scripting.

## 01_02 Pipes and redirection
| Stream | Name                     | Content                 |
| :---:  |    :----                 |  :---                   |
| 0      | Standard input (stdin)   | Keyboard or other input |
| 1      | Standard output (stdout) | Regular output          |
| 2      | STandard error (stderr)  | Output marked as 'error'| 

```bash
cat file.txt | wc -1  #show the lines, words, characters in file.txt
ls > file.txt
ls /notReal 1>output.txt 2>error.txt   #error will go to file error.txt if notReal does not exist
```
| Symbol | Function                   |
| :---:  |    :----                   |
|   >    | Output redirect (truncate) |
|   >>   | Output redirect (append)   |
|   <    | Input redirect (append)    |
|   <<   | Here document              |
```bash
cat <<LimitString
> take my money and
> give me chocolate
> LimitString
#Output 
#take my money and
#give me chocolate
```

## 01_03 builtin vs commands
```bash
command echo #runs command version
builtin echo #runs builtin version
```
builtin takes precedence over commands. you can tell if builtin is used or ```command -V echo```
But specific builtins can be disabled in a session with ```enable -n``` and the name of the builtin, allowing the command version to run instead. 
```bash
command -V echo
#Output echo is a shell builtin
enable -n echo 
command -V echo

#Output echo is /usr/bin/echo

  enable -n <command> #disable the builtin
  enable -n           #shows builtins that are disabled.
 
  enable <command>    #enable the builtin
 ```
## 01_05 Bash expansions and substitutions
```bash
echo ~    #represents home directory
echo ~-   #represents bash variabe $OLDPWD. shows previous directory
```

## 01_06 Brace expansion
```bash
echo {1..10}
echo {10..1}
echo {01..10}         # 1 to 10 padded with zeros
echo {01..100}            
echo a..z}
echo {Z..A}
echo {1..30..3}
echo {a..z..2}
touch file_{01..12}{a..d}
echo {cat,dog,fox}
echo {cat,dog,fox}_{1..5}
```

## 01_07 Parameter expansion
```bash
greeting="hello there!"
echo $greeting
echo ${greeting:6}
echo ${greeting:6:3}    # retrieve substring starting from char 6 uptil 3 chars
echo ${greeting/there/everybody}   #pattern substitution. replaces 'there' in greeting parameter with 'everybody'
echo ${greeting//e/_}              #Output h_llo th_r_!
echo ${greeting/e/_}               #Output h_llo there!
echo $greeting:4:3
```

## 01_08 Command substitution
Command substitution is a kind of special case expansion which allows us to use the output of a command within another command. This is represented by a dollar sign and a set of parentheses enclosing a command. An alternate way of writing this is ``` `...` ``` which often gets confusing. So generally, we write it with the parentheses ```&(...)```. Bash runs the specified command in a subshell and returns the output of that into the current command. It's often used together with string manipulation tools to extract a part of a command's output, such as a path, a file size, an IP address, or so on, that needs to be handed back up to the parent command. 
```bash
uname -r           #get the release version of kernal
echo "The kernel is $(uname -r)."
echo "Result: $(python3 -c 'print("Hello from Python!")' | tr [a-z] [A-Z])"
```
Command substitution is often used with tools such as GREP, AWK, and CUT, and is widely used to determine whether a script has what it needs on the target system.

## 01_09 Arithmetic expansion
```bash
echo $(( 2 + 2 ))
echo $(( 4 - 2 ))
echo $(( 4 * 5 ))
echo $(( 4 / 5 ))
```

## 02_01 Understanding Bash script syntax

There's two ways of running a Bash script. The first is to use the Bash command and the name of the script. That tells Bash to run the contents of the text file. When we do this, it's common to add a .sh extension on the end of the file name, so we can tell at a glance that a file is a shell script, but that's optional. 
```bash myScript.sh```
The other way, which is generally more common is to make an executable script which can be run just like any other program. The first step here is to add a line at the top of the script called the **shebang** line which tells the shell what program to use when the script is run. A shebang line starts with a pound or hash sign and an exclamation mark, and then the full path to whatever program should run the script. In this case, we'll use Bash but this can be anything like Python or Ruby or whatever interpreted scripting language you need to make into a program. It's common to see the absolute path to Bash in a shebang line in older script like, slash bin slash Bash but it's often considered a best practice to write the shebang line in a way that uses the shell's environment to locate the Bash executable. Not all systems have Bash in the same place though almost all of them do. And so instead of asking for bin, Bash and not finding it in that file path, we ask the environment to give us Bash which will work wherever Bash is installed. 

```bash
nano myScript.sh

#!/usr/bin/env bash
echo "hello"

# This is a comment
echo "there"

chmod +x myScript.sh
./myscript
```

## 02_03 Displaying text with 'echo'
```bash
echo hello world
worldsize=big
echo hello $worldsize world
echo "The kernel is $(uname -r)"
echo The kernel is $(uname -r)
echo The (kernel) is $(uname -r)    #Output: bash: syntax error near unexpected token `('
echo The \(kernel\) is $(uname -r)  #escaping a character. Output: The (kernel) is 6.2.0-1019-azure
echo 'The kernel is $(uname -r)'    #Strong quote, everyting inside ' ' is treaded as literal text
echo "The (kernel) is $(uname -r)"  #
echo "The (kernel) is \$(uname -r)" #conver $ to literal
echo
echo; echo "More space!"; echo
echo -n "No newline"
echo -n "Part of "; echo -n "a statement"
```
```echo -n``` give output without new line. This is useful when combining two echo results in a single line.

## 02_04 Working with variables
NOTE: no space on equal side of =
```bash
#!/usr/bin/env bash

mygreeting=hello
mygreeting2="Good Morning"
number=6

echo $mygreeting
echo $mygreeting2
echo $number
```

```bash
#!/usr/bin/env bash

myvar="Hello!"
echo "The value of the myvar variable is: $myvar"
myvar="Bonjour!"
echo "The value of the myvar variable is: $myvar"

declare -r myname="Scott"  #readonly declaration
echo "The value of the myname variable is: $myname"
myname="Michael"
echo "The value of the myname variable is: $myname"

declare -l lowerstring="This is some TEXT!"   #change to all lowercase
echo "The value of the lowerstring variable is: $lowerstring"
lowerstring="Let's CHANGE the VALUE!"
echo "The value of the lowerstring variable is: $lowerstring"

declare -u upperstring="This is some TEXT!"   #change to all uppercase
echo "The value of the upperstring variable is: $upperstring"
upperstring="Let's CHANGE the VALUE!"
echo "The value of the upperstring variable is: $upperstring"
```
Some useful commands about variables
```bash
declare -p   #display the attributes and value of each NAME
env          #this list all the environment variables
echo $USER   #example, show username
```

## 02_05 Working with numbers
1- ```$((...))``` Arithmetic expansion, return the result of mathematical operations.
2- ```((...))``` Arithmetic evaluation, performs calculations and changes the value of variables.

| Operation      | Operator |
| :---:          | :---:    | 
| Addition       | +        |
| Subtraction    | -        | 
| Multiplication | *        | 
| Division       | /        | 
| Modulo         | %        | 
| Exponentiation | **       | 

NOTE: Works with integers. Shorthands like +=, -=, *=, /= are available.
```bash
echo $((4+4))
echo $((8-5))
echo $((2*3))
echo $((8/4))
echo $(( (3+6) - 5 * (5-2) ))
a=3
((a+=3))
echo $a
((a++))
echo $a
((a++))
echo $a
((a--))
echo $a
(($a++))
((a++))
echo $a
a=$a+2
echo $a
declare -i b=3
b=$b+3
echo $b
echo $((1/3))
declare -i c=1
declare -i d=3
e=$(echo "scale=3; $c/$d" | bc)
echo $e
echo $RANDOM
echo $(( 1 + $RANDOM % 10 ))
echo $(( 1 + $RANDOM % 20 ))
```





## 02_06 Comparing values with test

When we run a comparison or test, the result we get back is an exit or return status of a zero or a one. 
* Zero for success and one for failure. zero indicates that a program finished running successfully. And one or any other number indicates that there was an error of some kind. 
* This exit status lets us do is treat the expression as a truth value. 
* That becomes important when we combine these tests and comparisons with flow control structures. 
* And we can read the return status of the most recently run command, by looking at the ```$?```

```bash
help test
[ -d ~ ]
echo $?
[ -d /bin/bash ]; echo $?
[ -d /bin ]; echo $?
[ "cat" = "dog" ]; echo $?  #compare strings 
[ "cat" = "cat" ]; echo $?
[ 4 -lt 5 ]; echo $?        # 4 < 5, result 0 i.e. TRUE
[ 4 -lt 3 ]; echo $?        # result 1, FALSE
[ ! 4 -lt 3 ]; echo $?      # invert a test by using ! 
[ -z $myVar ]               # check if myVar is empty
```
NOTE: spaces are important

## 02_07 Comparing values with extended test
```bash
[[ 4 -lt 3 ]]; echo $?
[[ -d ~ && -a /bin/bash ]]; echo $?    # && logical AND
[[ -d ~ && -a /bin/mash ]]; echo $?
[[ -d ~ || -a /bin/bash ]]; echo $?    # || logical OR
[[ -d /bin/bash ]] && echo ~ is a directory  # run command conditionally
ls && echo "listed the directory"
true && echo "success!"
false && echo "success!"
[[ "cat" =~ c.* ]]; echo $?            # test using regular expression
[[ "bat" =~ c.* ]]; echo $?
```

## 02_08 Formatting and styling text output
```bash
echo -e "Name\t\tNumber"; echo -e "Scott\t\t123"
echo -e "This text\nbreaks over\nthree lines"
echo -e "\a"
echo -e "Ding\a"
```

```bash
#!/usr/bin/env bash
echo -e "\033[33;44mColor Text\033[0m"
echo -e "\033[30;42mColor Text\033[0m"
echo -e "\033[41;105mColor Text"
echo "some text that shouldn't have styling"
echo -e "\033[0m"
echo "some text that shouldn't have styling"
echo -e "\033[4;31;40mERROR:\033[0m\033[31;40m Something went wrong.\033[0m"
```

```bash
#!/usr/bin/env bash

ulinered="\033[4;31;40m"
red="\033[31;40m"
none="\033[0m"

echo -e $ulinered"ERROR:"$none$red" Something went wrong."$none
```

## 02_09 Formatting output with printf
```%d``` or ```%s``` represent the placeholder for dynamic value, and gets presented in the same sequence of values.
If there are more values, it repeats the output for each input its given.
```bash
echo "The results are: $(( 2 + 2 )) and $(( 3 / 1 ))"
printf "The results are: %d and %d\n" $(( 2 + 2 )) $(( 3 / 1 ))
```

### Aligning text
```bash
#!/usr/bin/env bash

echo "----10----| --5--"

echo "Right-aligned text and digits"
printf "%10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned digits"
printf "%-10s: %5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text and digits"
printf "%-10s: %-5d\n" "A Label" 123 "B Label" 456

echo "Left-aligned text, right-aligned and padded digits"
printf "%-10s: %05d\n" "A Label" 123 "B Label" 456

echo "----10----| --5--"
```
### Date formatting
```bash
printf "%(%Y-%m-%d %H:%M:%S)T\n" 1658179558
date +%s
date +%Y-%m-%d\ %H:%M:%S
printf "%(%Y-%m-%d %H:%M:%S)T\n" $(date +%s)
printf "%(%Y-%m-%d %H:%M:%S)T\n"
printf "%(%Y-%m-%d %H:%M:%S)T is %s\n" -1 "the time"
```

## 02_10 Working with arrays

```bash
declare -a snacks=("apple" "banana" "orange")
echo ${snacks[2]}
snacks[5]="grapes"
snacks+=("mango")     #append to the end of the array
echo ${snacks[@]}
for i in {0..6}; do echo "$i: ${snacks[$i]}"; done

# associative arrays with key value pairs
declare -A office
office[city]="San Francisco"
office["building name"]="HQ West"
echo ${office["building name"]} is in ${office[city]}"
```

## 03_01 Conditional statements with the 'if' keyword

```bash
#!/usr/bin/env bash

declare -i a=3

if [[ $a -gt 4 ]]; then
    echo "$a is greater than 4!"
else
    echo "$a is not greater than 4!"
fi
```

```bash
#!/usr/bin/env bash

declare -i a=3

if [[ $a -gt 4 ]]; then
    echo "$a is greater than 4!"
elif (( $a > 2 )); then
    echo "$a is greater than 2."
else
    echo "$a is not greater than 4!"
fi
```

## 03_02 Working with while and until loops
```bash
#!/usr/bin/env bash

echo "While Loop"

declare -i n=0
while (( n < 10 ))
do
    echo "n:$n"
    (( n++ ))
done

echo -e "\nUntil Loop"
declare -i m=0
until (( m == 10 )); do
    echo "m:$m"
    (( m++ ))
done
```

## 03_03 Introducing 'for' loops
```bash
#!/usr/bin/env bash

for i in 1 2 3
do
    echo $i
done

for i in 1 2 3; do echo $i; done
```

```bash
#!/usr/bin/env bash

for i in {1..100}
do
    echo $i
done
```

```bash
#!/usr/bin/env bash

for (( i=1; i<=100; i++ ))
do
    echo $i
done
```

```bash
#!/usr/bin/env bash

declare -a fruits=("apple" "banana" "cherry")
for i in ${fruits[@]}
do
    echo $i
done
```

```bash
#!/usr/bin/env bash

declare -A arr
arr["name"]="scott"
arr["id"]="1234"
for i in "${!arr[@]}"
do
    echo $i: "${arr[$i]}"
done
```

```bash
#!/usr/bin/env bash

for i in $(ls)
do
    echo "Found a file: $i"
done
```

```bash
#!/usr/bin/env bash

for i in *
do
    echo "Found a file: $i"
done
```

## 03_04 Selecting behavior using 'case'
```bash
#!/usr/bin/env bash
animal="dog"
case $animal in
    bird) echo "Avian";;
    dog|puppy) echo "Canine";;
    *) echo "No match!
esac
```

## 03_05 Using functions
```bash
#!/usr/bin/env bash

greet() {
    echo "Hi there."
}

echo "And now, a greeting..."
greet
```

```bash
#!/usr/bin/env bash

greet() {
    echo "Hi there, $1"
}

echo "And now, a greeting..."
greet Scott
```

```bash
#!/usr/bin/env bash

greet() {
    echo "Hi there, $1. What a nice $2"
}

echo "And now, a greeting..."
greet Scott Morning
greet Everybody Evening
```
```$@```: represents the list of arguments given to a function
```$#```: Get the number of arguments passed to the bash script
```$FUNCNAME```: 
```bash
#!/usr/bin/env bash

numberthing() {
    declare -i i=1
    for f in $@; do
        echo "$i: $f"
        (( i += 1 ))
    done
    echo "This counting was brought to you by $FUNCNAME."
}

numberthing "$(ls /)"
echo
numberthing pine birch maple spruce
```

```bash
#!/usr/bin/env bash

var1="I'm variable 1"

myfunction() {
    var2="I'm variable 2"
    local var3="I'm variable 3"
}
myfunction

echo $var1
echo $var2
echo $var3
```

## 03_06 Reading and writing text files
### Overwriting
```bash
#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" > ~/textfile.txt
done
```

### Appending
```bash
#!/usr/bin/env bash

for i in 1 2 3 4 5
do
    echo "This is line $i" >> ~/textfile.txt
done
```

```bash
#!/usr/bin/env bash

while read f
    do echo "I read a line an it says: $f"
done < ~/textfile.txt
```

## 04_01 Working with arguments

```$0```: Got the script name

```bash
#!/usr/bin/env bash

echo "The $0 script got the argument $1"
```

```bash
#!/usr/bin/env bash

echo "The $0 script got the argument $1"
echo "Argument 2 is $2"
```

```bash
#!/usr/bin/env bash

for i in $@
do
    echo $i
done

echo "There were $# arguments."
```

## 04_02 Working with options
```getopts```: is a keyword

```bash
#!/usr/bin/env bash

while getopts u:p: option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
    esac
done

echo "user: $user / pass: $pass"
```

```bash
#!/usr/bin/env bash

while getopts u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
    esac
done

echo "user: $user / pass: $pass"
```

```bash
#!/usr/bin/env bash

while getopts :u:p:ab option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "got the A flag";;
        b) echo "got the B flag";;
        ?) echo "I don't know what $OPTARG is!"
    esac
done

echo "user: $user / pass: $pass"
```

## 04_03 Getting input during execution
```bash
#!/usr/bin/env bash

echo "What is your name?"
read name
echo "What is your password?"
read -s pass
read -p "What's your favorite animal? " animal

echo "name: $name, pass: $pass, animal: $animal"
```
```bash
help read
```

```bash
#!/usr/bin/env bash

echo "Which animal"
select animal in "bird" "dog" "bird" "fish"
do
    echo "You selected $animal!"
    break
done
```

```bash
#!/usr/bin/env bash

echo "Which animal"
select animal in "bird" "dog" "quit"
do
    case $animal in
        bird) echo "Birds like to fly.":;;
        dog) echo "Dogs like to play catch.";;
        quit) break;;
        *) echo "I'm not sure what that is.";;
    esac
done
```

## 04_04 Ensuring a response
```bash
#!/usr/bin/env bash

read -ep "Favorite color? " -i "Blue" favcolor
echo $favcolor
```

```bash
#!/usr/bin/env bash

if (($#<3)); then
    echo "This command requires three arguments:"
    echo "username, userid, and favorite number."
else
    # the program goes here
    echo "username: $1"
    echo "userid: $2"
    echo "favorite number: $3"
fi
```

```bash
#!/usr/bin/env bash

read -p "Favorite animal? " fav
while [[ -z $fav ]]
do
        read -p "I need an answer! " fav
done

echo "$fav was selected."
```

```bash
#!/usr/bin/env bash

read -p "Favorite animal? [cat] " fav
if [[ -z $fav ]]; then
        fav="cat"
fi
echo "$fav was selected"
```

```bash
#!/usr/bin/env bash

read -p "What year? [nnnn] " year
until [[ $year =~ [0-9]{4} ]]; do
        read -p "A year, please! [nnnn] " year
done
echo "Selected year: $year"
```

## Troubleshooting Tips
1- Check closure of expensions and substitutions 
2- Variables are case sensetive.
3- Use set -x to display commands as they run. And, to turn it off, you can use "set +x".
4- Add echo statements to keep track of program flow.
