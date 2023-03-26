---
layout: post
title:  "A quick review of Bash"
date:  2022-10-10T11:32:00+0800
tags: ["review_label"]
categories: ["Programming"]
keywords: bash shell
author: Yichong Zhou
hidden: false
---

> The shell is the Linux command line interpreter that executes programs called commands. It provides an interface between the user and the kernel. Although not a powerful scripting language, a shell script could be a quick-and-dirty method of prototyping a complex application. Note that most of the materials in this post are collected from internet. 



<!--more-->


{% include toc.html %}


## Introduction

Shell scripts are interpreted, not compiled. 
The shell reads commands from the script line per line and searches for those commands on the system. 
Bash is an sh-compatible shell that incorporates features includes 
command line editing, unlimited size command history, job control, shell functions and aliases, 
indexed arrays of unlimited size, and integer arithmetic in any base from 2 to 64. 
The restricted shell rbash is invoked with the `--restricted` or `-r` option. 

When the program being executed is a shell script, bash will create a new bash process using a *fork*. This
subshell reads the lines from the shell script one line at a time. Commands on each line are read, interpreted
and executed as if they would have come directly from the keyboard.
While the subshell processes each line of the script, the parent shell waits for its child process to finish. When
there are no more lines in the shell script to read, the subshell terminates. The parent shell awakes and
displays a new prompt.


## Examples


```bash
#!/bin/bash
echo "$@" "$#"
for i in "$@" ; do echo "$i" ; done
[ $1 -eq "0" ] && echo "ok"

run(){ local i ; i=10 ; j=$(($i+1)) ; echo $j ; }
wraper(){ "$@" ; echo "wraper done" ; }
wraper run  # 11 // wraper done

var_37=lolilol ; i=37 ; name="var_$i"
echo "$name"         # var_37
echo "${!name}"      # lolilol
echo "${!name%lol}"  # loli

get_random(){
    local d ; d=$((RANDOM % 100)) ; s=$(($1-1))
    if [ $# -gt "0" ] ; then 
      [ $d -le '60' ] && shuf -i 0-$s -n 1
      [ $d -gt '60' ] && echo $((RANDOM % $1))
    else
      shuf -i 1-100 -n 1 
    fi
}

list_path=/home/user/Music/playlist ; let x=$(grep -c ^ $list_path)  # 19
for (( c=1; c<=$x; c++ )); do 
  sed -n ${c}'p' $list_path | grep -oP '[^\/]+\/[^\/]+(?!\/[^\/]+)$' | xargs echo $c":"; done
nohup vlc $(sed -n ${selected}'p' $list_path) >/dev/null 2>&1 &
selected='0' ; re='^[0-9]+$' ; ! [[ $selected =~ $re ]] && selected='1' ; echo $selected  # 0

gnome-terminal -- bash -c "$@"
gnome-terminal -- "$@"
watch -n 1 date
let STATE=$(cat $CONFIG_PATH/state.txt) ; S_L=() ; S_L+=("$STATE") 
echo $1 > $CONFIG_PATH/state.txt && echo $(date) "Change state to $1 from $STATE..." >> $CONFIG_PATH/log.txt

export DISPLAY=:0.0 && echo DISPLAY=$DISPLAY
pname="vlc-"$s; PULSE_SINK="${SINKS[$i]}" exec -a $pname vlc $1 >/dev/null 2>&1 &  
C_DIR=$(dirname "$0") ; source $C_DIR/paths.sh
n=$(wc -l < $LOG_PATH/log.txt)
date '+%Y-%m-%d %H:%M:%S %Z'
pacmd list-sinks | grep -e name: -e index -e description -e muted -e volume:

let x=1+1; echo $x  # 2  # x=1+1; echo $x  # 1+1
cat /etc/shells  # 
man bash; help for; man echo
```



## Features



### Conditions

```bash
# Note that there are always spaces between the brackets and the logical expression. 
# It won't work without them, e.g., if [$foo -ge 3]; then 
# keywords: if, then, else, elif, fi; while, until

# Single-bracket syntax
if [ "$str_var" == "cheese" ]; then  # true if '$str_var' contains just the string "cheese"
if [ "$str_var" == "tux" ]; then  # quote string variables as they may contain spaces and/or newlines.
if [ ! -f regularfile ]; then  # invert a condition
if [ $foo -ge 3 -a $foo -lt 10 ]; then  # '-a' for 'and' and '-o' for 'or'.
if [ -L symboliclink_file ]; then  # true if the file 'symboliclink_file' exists and is a symbolic link.
if [ -z "$emptystring" ]; then  # true if $emptystring is an empty string or an uninitialized variable.
if [ $num -lt 1 ]; then  # arithmetic conditions

# Double-bracket syntax
if [[ "$str_var" == *cheese* ]]; then  # true if "$str_var" contains the phrase "cheese" anywhere
if [[ "$str_var" == *[cC]heese* ]]; then  # match "cheese" or "Cheese"
if [[ $stringvarwithspaces != foo ]]; then  # it works without quotes
# if [ -a *.sh ]; then  # true if there is one single '.sh' file in the working directory 
# error if thera are more than one '.sh' files 
# if [[ -a *.sh ]]; then  # true only if there is a file in the working directory called '*.sh'
# 'The asterisk is taken literally, because the double-bracket syntax does not expand filenames. 
if [[ $num -eq 3 && "$str_var" == foo ]]; then #  '&&' and '||' are supported (as '-a' and '-o')
# the double-bracket syntax allows regex pattern matching using the '=~' operator

# String-based conditions
[ STR1 == STR2 ] [ STR1 != STR2 ] [ STR1 > STR2 ] [ STR1 < STR2 ] [ -n NONEMPTYSTR ] [ -z EMPTYSTR ] 
[[ STR =~ REGEX_PATTERN ]]  # Double-bracket syntax only

# Arithmetic conditions
[ NUM1 -eq NUM2 ] [ NUM1 -ne NUM2 ] [ NUM1 -gt NUM2 ] [ NUM1 -ge NUM2 ] [ NUM1 -lt NUM2 ] [ NUM1 -le NUM2 ]
# Double-parenthesis syntax
if (( $num <= 5 )); then  # syntax for arithmetic (number-based) conditions
# It supports '==', '<', '>=', '&&' and '||' (but not the '-a' and '-o').
(( NUM1 == NUM2 )) (( NUM1 != NUM2 )) (( NUM1 > NUM2 )) (( NUM1 >= NUM2 )) (( NUM1 < NUM2 )) (( NUM1 <= NUM2 ))

# File-based conditions
[ -a existingfile ] [ -e existingfile ] [ -b blockspecialfile ] [ -c characterspecialfile ] [ -d directory ]
[ -f regularfile ] # A regular file is neither a block or character special file nor a directory. 
# e.g., if [ -f ~/.bashrc ]; then source ~/.bashrc; fi
[ -w writeablefile ] [ -x executablefile ] [ -r readablefile ] [ -s nonemptyfile ] 
[ -h symboliclink ] [ -L symboliclink ] [ -S socket ] [ -t openterminal ] 
[ -g sgidfile ] [ -G fileownedbyeffectivegroup ] [ -k stickyfile ] [ -N modifiedsincelastread ] 
[ -p namedpipe ] [ -u suidfile ] [ newerfile -nt olderfile ] [ olderfile -ot newerfile ] 
[ same -ef file ] [ -O fileownedbyeffectiveuser ]
# Other conditions
[ -o shell_option ]  # true if shell option 'shell_option' is enabled.

# case statement
case $COUNTRY in
  "Lithuania")
    echo "Lithuanian" ;;
  Romania | Moldova)
    echo "Romanian" ;;
  *)
    echo "unknown" ;;
esac
```


### Loops



```bash
for i in {1..5}; do echo -n $i' '; done; echo ''  # 1 2 3 4 5 
for((i=1;i<=6;i+=1)); do echo -n $i' '; done; echo ''  # 1 2 3 4 5 6 
for i in *; do echo "$i"; done  # print all the files in the current directory
for i in *.md; do cp "$i" /backup; done  # for i in *.txt; do echo "$i"; done
for var in one two three; do echo -n "$var "; done; echo ""  # one two three 
for i; do echo -n "$i "; done; echo ""  # "$@" is assumed, i.e., for i in "$@" 
dirs="/home/vivek /etc /usr/local/etc"; for b in $dirs; do echo "Backing up $b..."; done
for s in $(cat test.txt); do echo "$s "; done; echo ''
dir="/etc"; for file in "${dir}"/a*.conf; do echo "$dir contains $file"; done
dir="/etc"; for file in $(ls "${dir}"/a*.conf); do echo "$dir contains $file"; done

x=0; while [ $x -le 5 ]; do (( x++ )) ; echo -n "$x "; done; echo ''  # 1 2 3 4 5 6  # x=$(( $x + 1 ))


```

`getopts` processes one option per loop iteration, it returns false if there are no more options to be processed. 
The option's value is in the `$OPTARG` if the option is expecting an argument. 
`optstring` is a string which defines what options and arguments getopts look for.
`$OPTARG` is set to `':'` if an expected argument is not found.
It usually processes the arguments in `$@`. 
To manually provide arguments for `getopts` to parse, specify these `[args]` as the final argument. 

```bash
# getopts optstring optname [args]
getopts "apZ" optname  # '-a', '-p', and '-Z', with no arguments, are expected. 
getopts "a:pZ:" optname  # '-a' and '-Z' take arguments, '-p' has no argument 
# A colon at the beginning of the optstring means "silent error checking mode". 
# i.e., it will not report any verbose errors about options or arguments
while getopts ":x:y:" opt; do
  case $opt in
    x) echo "x is $OPTARG" ;;
    y) echo "y is $OPTARG" ;;
    *) echo "invalid option or argument $OPTARG" ;;
  esac
done

# Transform long options to short ones
for arg in "$@"; do
  shift
  case "$arg" in
    '--help')   set -- "$@" '-h'   ;;
    '--number') set -- "$@" '-n'   ;;
    *)          set -- "$@" "$arg" ;;
  esac
done
# 'shift' is a bash built-in which removes arguments from the beginning of the argument list. 
# Given that the '3' arguments provided to the script are available in '$1', '$2', '$3', 
# then a call to 'shift' will make '$2' the new '$1'. A 'shift 2' will shift by 2 making new '$1' the old '$3'.
```

By default, getopts will report a verbose error if it finds an unknown option or a misplaced argument. It also sets the value of optname to a question mark ("?"). It does not assign a value to $OPTARG.

If the option is valid but an expected argument is not found, optname is set to "?", $OPTARG is unset, and a verbose error message is printed.

In silent mode, if an option is unexpected, getopts sets optname to "?" and $OPTARG to the unknown option character.

If the option is OK but an expected argument is not found, optname is set to a colon (":") and $OPTARG is set to the unknown option character.





### Piping & Redirecting Streams

Use the `|` to pipe the content that previous command writes to the *stdout* to the *stdin* for next command. 
Use the `|&` instead to pipe both the *stderr* and *stdout* to the next command. 

```bash
echo "hello world" | grep hello
echo "hello world" |& cat

echo "hello world" > hello.txt  # overwrite
echo "hello world" >> hello.txt  # append
cat < hello.txt  # take input from file  # cat hello.txt
python3 pyin.py < hello.txt  # send the file as input for the script
python3 pyin.py < hello.txt > output.txt
notacommand 2> error.txt  # redirect the content of the stderr to a file
echo "hello world" 1>output.log 2>debug.log  # the file descriptor 1 and 2 before the redirection is needed
```


### Variables

Variables are created with the `declare` built-in command.
If no value is given, a variable is assigned the null string. Variables can only be removed with the `unset` built-in. 
A parameter is an entity that stores values. It can be a name, a number or a special value. 
For the shell's purpose, a variable is a parameter that stores a name. A variable has a value and zero or more attributes.




### Arrays


Bash provides one-dimensional array variables. Any variable may be used as an array; the declare built-in
will explicitly declare an array. There is no maximum limit on the size of an array, nor any requirement that
members be indexed or assigned contiguously. Arrays are zero-based. 



### Functions

Shell functions are a way to group commands for later execution using a single name for the group. They are
executed just like a "regular" command. When the name of a shell function is used as a simple command
name, the list of commands associated with that function name is executed. 
Shell functions are executed in the current shell context; no new process is created to interpret them. 


### Internal Commands / Builtins

Internal commands or builtins are part of the shell, a particular builtin needs direct access to the shell internals. Instead of loading an external program and forking off a separate process, they are executed directly in the shell, therefore faster than external commands. 

```bash
help ; help | less ; help | grep read  # see all bash builtins, e.g., 

# Bourne Shell built-ins:
:, ., break, continue, return, getopts, cd, eval, exec, exit, export, set, shift, test, [, 
hash, pwd, readonly, times, trap, umask and unset.
# Bash built-ins:
echo, help, let, local, printf, read, alias, bind, builtin, command, declare, enable, logout, shopt,
type, typeset, ulimit and unalias.
: # (a colon)
. # (a period)
break [n]  # Exit from a for, while, until, or select loop. 
# If 'n' is supplied, the nth enclosing loop is exited. 
continue [n]  # Resume the next iteration of an enclosing for, while, until, or select loop. 
# If 'n' is supplied, the execution of the nth enclosing loop is resumed.

shift [n]; set [-abefhkmnptuvxBCEHPT] [-o option-name] [--] [-] [argument…]
# 'shift' removes arguments from the beginning. e.g., "./a.sh -x -y -z"
echo "$@"     # output: 
shift         # -x -y -z
echo "$@"     # -y -z
# 'shift' rotates arguments by using 'set -- "$@" "$arg"'. e.g., "./a.sh -x test -y a --help"
for arg in "$@"; do                                              # output: 
  shift  # shift 1                                               # test -y a --help -x @*
  case "$arg" in                                                 # -y a --help -x test @*
    '--help')   set -- "$@" '-h' ; echo "$@" '@--help'  ;;       # a --help -x test -y @*
    '--number') set -- "$@" '-n' ; echo "$@" '@--number'  ;;     # --help -x test -y a @*
    *)          set -- "$@" "$arg" ; echo "$@" '@*' ;;           # -x test -y a -h @--help
  esac
done   
# The '$1' and the 'shift' process each argument
while (( "$#" )); do  # In a 'while' loop with a condition of '(( $# ))'. 
  if [[ $(ls "$1") == "" ]]; then  
     echo "Empty directory."
  else 
     find "$1" -type f -a -atime +365 -exec rm -i {} \;
  fi     # '$#' is reduced each time 'shift' is executed and eventually becomes zero,
  shift  # upon which the 'while' loop exits. The condition is true as long as '$#' is greater than zero. 
done     

```

Directory stack: The `pushd` built-in adds directories to the stack and the `popd` built-in removes directories from the stack, 
both of which change the current directory. `dirs` display the content of the stack. 



## Linux 


### File Descriptors (FD)

In Linux, almost all the streams are treated as if they were files. <!-- Just like you can read/write a file, you can read/write data from these streams.. -->
Regular file, directories, and even devices are files. 
Every File has an associated number called File Descriptor (FD). 

The screen has a file descriptor. The output can also be sent to the file descriptor of a printer to print. 

`0`: *stdin* - standard input stream; `1`: *stdout* - standard output stream; `2`: *stderr* - standard error stream; 


```bash
# search for a string 'hello' in the "/sys" directory, which results in "Permission denied" errors
grep -r hello /sys/ > /dev/null 2>&1  # dump all the output into the void using redirection '> /dev/null 2>&1'
# dump all the stdout to '/dev/null', then, send stderr 2 to stdout 1.
grep -r hello /sys/ &> /dev/null  # both stdout and stderr will be redirected to '/dev/null'
```



### External Commands


The `getopt` command parses command-line options preceded by a dash. This external command corresponds to the `getopts` Bash builtin. 
Using `getopt` permits handling long options by means of the `-l` flag, and this also allows parameter reshuffling.

```bash
# getopt
# grep, expr, sed and awk, interpre

```





## References & Resources  {#references}


*\[1\]. [Bash (Unix shell) - wikipedia][1]*
{: id="ref-1-bash-wikipedia" class="ref-item" }

*\[2\]. [shellcheck.net][2]*
{: id="ref-2-shellcheck-net" class="ref-item" }

*\[3\]. [Conditions in bash scripting][3]*
{: id="ref-3-conditions-in-bash-scripting-if-statements" class="ref-item" }

*\[4\]. [Input Output Redirection in Linux/Unix][4]*
{: id="ref-4-input-output-redirection" class="ref-item" }

*\[5\]. [What are stdin, stderr and stdout in Bash][5]*
{: id="ref-5-bash-stdin-stderr-stdout" class="ref-item" }

*\[6\]. [The Bash getopts builtin][6]*
{: id="ref-6-the-bash-getopts-builtin" class="ref-item" }

*\[7\]. [The shift built-in][7]*
{: id="ref-7-the-shift-built-in" class="ref-item" }

*\[8\]. [Advanced Bash-Scripting Guide][8]*
{: id="ref-7-advanced-bash-scripting-guide" class="ref-item" }

*\[9\]. [Bash Guide for Beginners][9]*
{: id="ref-7-bash-beginners-guide" class="ref-item" }



[1]: <https://en.wikipedia.org/wiki/Bash_(Unix_shell)/>  "Bash (Unix shell) - wikipedia"

[2]: <https://www.shellcheck.net/>  "shellcheck.net"

[3]: <https://acloudguru.com/blog/engineering/conditions-in-bash-scripting-if-statements/>  "Conditions in bash scripting (if statements)"

[4]: <https://www.guru99.com/linux-redirection.html>  "Input Output Redirection in Linux/Unix"

[5]: <https://linuxhint.com/bash_stdin_stderr_stdout/>  "What are stdin, stderr and stdout in Bash"

[6]: <https://stackoverflow.com/questions/12022592/how-can-i-use-long-options-with-the-bash-getopts-builtin/>  "The Bash getopts builtin"

[7]: <https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_07.html>  "The shift built-in"

[8]: <https://tldp.org/LDP/abs/html/>  "Advanced Bash-Scripting Guide"

[9]: <https://tldp.org/LDP/Bash-Beginners-Guide/html/index.html>  "Bash Guide for Beginners"








