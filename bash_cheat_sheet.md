# Bash Cheat Sheet

## Contents
**[Internal Variables](#Internal Variables)**  
**[Conditionals](#Conditionals)**  


## Internal Variables
| Variable (follows $) | Usage |
| --- | --- |
| PWD | Present Working Directory |
| HOME | Home directory of user |
| HOSTNAME | Gets system host name |
| HOSTTYPE | identifies system hardware |
| IFS | Internal Field Separator. Default is whitespace (space, tab, and newline)* |
| LINENO | Linenumber of the shell script in which this variable appears. Useful primarily for debugging. |
| OSTYPE | Operating System |
| PATH | Path to binaries |
| PPID | ID (pi) of parent proces |
| PS1 | Main prompt, seen at the command line |
| PS2 | Secondary prompt, when additional input is expected. Displays as ">" |
| SECONDS | number of seconds the script has been running |
| 0,1,2,...,n | arguments passed with script. 0 is the script itself. |
| # | The number of command-line arguments |
| * | All of the positional parameters, seen as a single word. Must be quoted "$*" |
| @ | Same as $* but each parameter is a quoted string, that is, the parameters are passed on intact, without interpretation or expansion. |
| - | Flags passed to script (using set) | 

## Conditiionals
### Syntax
```
if [ a -operator b ] && [ a -operator b ] # [[ condition && condition ]] works too, but not with single []
# ^ ^           ^ ^ ^  ^ ...etc. spaces matter
then
    do stuff
elif [ a -operator b ]
    do more stuff
else
    final stuff
fi
```
* [[]] should be used over [] if bash version is new enough. It's more versatile and safer.
    * Handles logical operators better
    * can compare decimal/hex/octal values without conversion
    * can do pattern matching for strings (ex: [[ $a == z* ]]

### Operators
#### File Comparisons
| Operator | Usage |
| --- | --- |
| -e | true if exists |
| -f | true if exists and is regular file |
| -d | true if exists and is a directory |
| -L | true if exists and is symlink |
| -r | true if exists and is readable |
| -w | true if exists and is writable |
| -x | true if exists and is executable |

#### Integer/String Comparisons
| Integer operator | String Operator | Usage |
| --- | --- | --- |
| -eq | = or ==| is equal to |
| -ne | != | is not equal to |
| -gt | \> (alphabetical) | greater than |
| -ge | N/A | greater than or equal to |
| -lt | \< (alphabetical) |less than |
| -le | N?A | less than or equal to |
| N/A | -z | String is NULL |
| N?A | -n | String is not NULL |

#### Logical Operators
| Operator | Usage |
| --- | --- |
| ! | NOT |
| \|\| | OR |
| && | AND |

## Loops
#### C-like for loop
```
for index in `seq 1 10`;
do
    do stuff
done
```
#### For loop through array
```
for index in "${array[@]}"
do
    do stuff
done
```
#### While loop
```
while [ expression ]; do # ; acts as new line, allowing 'do' to be on the same line
  do stuff
done
```
## Case Statement
```
case "$1" in
  option1)
    do stuff
    ;; # break
  option2)
    shift # removes "$1" and shifts all other arguments left 1
    do stuff
    ;;
  option3)
    shift
    do stuff
    ;;
  *)
    do default behavior
    exit 1
    ;;
esac
```

## Parsing
| Operator | Usage |
| --- | --- |
| ${parameter#pattern} | remove the shortest possible string that matches the pattern from the beginning |
| ${parameter##pattern} | remove the longest possible string that matches the pattern from the beginning |
| ${parameter%pattern} | remove the shortest possible string that matches the pattern from the end |
| ${parameter%%pattern} | remove the longest possible string that matches the pattern from the end |
| ${parameter/pattern/replacement} | replaces the FIRST instance of pattern with replacement |
| ${parameter//pattern/replacement} |  same but replaces everything |
| ${#parameter} |  expands the length of the variable (in bytes) |
| ${parameter:start[:length]} | show bytes starting at start for all of length |
| ${parameter[^|^^|,|,,][pattern]} | change to upper case or lower case. ^ is upper and , is lower. Double means for every instance in variable |

## Redirection


## Functions
```
function function_name {
}
