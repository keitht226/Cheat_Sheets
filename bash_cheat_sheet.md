# Bash Cheat Sheet

## Contents
**[Internal Variables](#Internal-Variables)**  
**[Conditionals](#Conditionals)**  

## Basics
* $ expands variables
* "$var1" '$var2' : var1 will expand, var2 will not
* ${var1} should pretty much always use curly braces. Clearer and safer.
* Data types are assigned dynamically.
    * `Array=( 'string' 'string' 'string' ) # spacing matters`
    * `int=5`
    * `boolean=true`
    * `float=0.0`


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
```bash
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
```bash
for index in `seq 1 10`;
do
    do stuff
done
```
#### For loop through array
```bash
for index in "${array[@]}"
do
    do stuff
done
```
#### While loop
```bash
while [ expression ]; do # ; acts as new line, allowing 'do' to be on the same line
  do stuff
done
```
## Case Statement
```bash
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
| ${parameter[^ or ^^ or , or ,,][pattern]} | change to upper case or lower case. ^ is upper and , is lower. Double means for every instance in variable |

## Redirection
```bash
# FD0: standard input FD1: Standard Output FD2: Standard Error
ls -l a b >myfiles.ls 2>/dev/null
# writes ls results for directories a,b to a file called myfiles.ls. Any errors
# will be trashed. 
# /dev/null is a trash directory. Anything sent there will disappear for good
# < can be used instead of > to read files. <> is used to read/write
# << >> appends rather than overwriting
 
ls -l a b >myfiles.ls 2>myfiles.ls # BAD!!!
# streams collide and result in a garbled mess

ls -l a b >myfiles.ls 2>&1 # GOOD!!!
# can be shortened to:
ls -l a b &>myfiles.ls
# works with >> <<
```

## Functions
```bash
function function_name {
    # ${1} will grab the first argument given to a function
}
or
function_name(){}
              ^ parenthesis should always be empty

function_name $arg1 $arg2 ...
```

## Reading Input (with parsing example)
* Command `read` is used.
### Read Options
| Option | Usage |
| --- | --- |
| -a ANAME | The words are assigned to sequential indexes of the array variable ANAME, starting at 0. All elements are removed from ANAME before the assignment. Other NAME arguments are ignore. |
| -d DELIM | The first character of DELIM is used to terminate the input line, rather than newline |
| -e | readline is used to obtain the line |
| -n NCHARS | read returns after reading NCHARS characters rather than waiting for a complete line of input |
| -p PROMPT | Display PROMPT, without a trailing newline, before attempting to read any input. The prompt is displayed only if input is coming from a terminal. |
| -r | If this option is given, backslash does not act as an escape character. The backslash is considered to be part of the line. In particular, a backslash-newline pair may not be used as a line continuation |
| -s | silent mode. If input is coming from a terminal, characters are not echoed |
| -t TIMEOUT | cause read to time out and return failure if a complete line of input is not read within TIMEOUT seconds. This option has no effect if read is not reading input from the terminal or from a pipe. |
| -u FD | read input from file descriptor FD | 
* ```while IFS=`` read -r line || [[ -n "$line" ]]``` 
    * ```IFS=`` ``` prevents leading/trailing whitespace from being trimmed
    * -r prevents backslash escapes from being interpreted
    * `|| [[ -n "$line" ]]` prevents the last line from being ignored if it doesn't end with a \n (read returns a non-zero exit code as soon as it encounters EOF)

### Example
```bash
#!/bin/bash
 
file="parsing_file.txt"
# parsing_file.txt contains the following:
# this file will help me learn how to parse
# learning to parse will be useful at work
# yay for !!!parse!!!!

# want to find the word parse in the above text
key_word="parse"
 
# this syntax is called command substitution. Old syntax is to use `stuff`
# instead of $(stuff)
file_path=$(find ~/ -name $file)
 
# read one line at a time of the file
count=0
while read line;do
 
  # delimeter default is a blank space. Can be changed to be anything else
  # by setting IFS system variable. ex: IFS=';' will set the delimeter to be a
  # semi-colon
  for i in ${line[@]};do
    # the paranthesis are important. Older method is 'let "count=count+1"
    ((count++))
    # e option enabled interpretation of backslash characters. 
    # echo -e "$count\t$i"
 
    # -eq is used instead of = for integer comparisons. It does not work for
    # strings
    # spaces after and before [] are important!!!!
    # =~ means we're using regular expressions. Used to extract the worde parse
    # from whatever string it might have been in.
    if [[ "${i}" =~ (parse) ]];then
      echo $count
      count=0
      break
    fi
  done
 
done <${file_path} # if reading from user, just leave as done
 
exit
```

### Example 2
```bash
while IFS=`` read -r line || [[ -n "$line" ]]
do
    read -r -a split_line <<< "$line"  #assigns each word of line into the array: split_line
done < $file_name
```

## Command Substitution
Used to assign output of shell commands to a variable
```bash
file_path=$(find ~/ -name $file)

or

file_path=`find ~/ -name $file` # old method

## Misc
* Shorthand test for failure
```bash
rm hello.txt || echo "Couldn't delete hello.txt." >&2
# || only perform the second command if the first fails
```
* Useful functions
```bash
in_array() { for e in "${@:2}"; do [[ "$e" = "$1" ]] && break; done;}
````
* Good to use when globbing. Prevents error in case the glob does not match any name
`shopt -s nullglob`
* 
