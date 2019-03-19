---
title: awk and gawk
tags: [shell]
keywords: Linux shell command awk and gawk
last_updated: March 18, 2019
summary: "Linux shell commands awk and gawk are pattern scanning and processing language"
sidebar: mydoc_sidebar
permalink: shell_awk.html
folder: shell
---
Command gawk is the GNU implementation of awk by adding some extra features. If gawk is installed, awk is just symbolic link to gawk.
The data processing commands are in C grammar.

## Basic Syntax
```bash
# awk [options] file ...
awk '{print}' data-file.txt

# command can be put into another file by the -f option
aws -f command-file.txt data-file.txt
```

## Standard Options

### -v (--assign)
Assign value to a variable. **The reference of variable does not need the "$" sign**.
```text
awk -v var=value 'BEGIN {printf("Name = %s\n", var)} {print}' data-file.txt
```

### --dump-variables[=file]
```bash
# the following command will output awk's environment variables to a default faile awkvars.out
aws --dump-variables ''

# the following command will output awk's environment variables after running commands
awk -v var=value --dump-variables 'BEGIN {printf("Name = %s\n", var)} {print}' data-file.txt
```
### -p (--profile)
Output commands to a file, the default file is awkprof.out. 
```bash
# --profile[=file]
awk -v var=value --profile 'BEGIN {printf("Name = %s\n", var)} {print}' data-file.txt
```

### --lint
This option enables checking of dubious constructs. When an argument fatal is provided, awk treats warnings as errors.
The first command will give you all the warnings of the current awk command; the second one will fail on the first warning and
you get a non-zero exit code.

```bash
awk --lint ''
awk --lint=fatal ''
```

## Regular Expression
### Pattern Matching
```text
# this matches all the lines that have "abc" in it
awk '/abc/' data-file.txt

# these should do the same thing as the previous command
awk '$0 ~ /abc/'
awk '$0 ~ /abc/ {print}'
awk '$0 ~ /abc/ {print $0}'

# not matching
awk '$0 !~ /abc/'

# case insensitve matching
awk 'BEGIN{IGNORECASE=1} $0 ~ /abc/ {print}'
```

### RE basics
* **.** - a single character
* **^** - start of a line
* **$** - end of a line
* **[ABC]** - character set
* **[^ABC]** - exclusive set
* **|** - ORed matching such as "ABCD | EFGH"
* **?** - zero or one occurrence
* \* - zero of more occurrences
* **+** - one or more occurrences
* (ABC | DEF) - grouping by parentheses
 
## Resources
* A [tutorial](https://www.tutorialspoint.com/awk/) on [Tutorial Point](https://www.tutorialspoint.com) on awk.