---
title: String
tags: [python]
keywords: python
last_updated: March 23, 2019
summary: "Python String"
sidebar: mydoc_sidebar
permalink: python-string.html
folder: python
---
## Basics
* Python string be enclosed by either double quotes or single quotes.
* If a string contains single quotes, then use double quotes to enclose string.
* use triple quotes ''' to define multi-line string.

## Substring
* use array accessor "string[index]" to get a single character in a string.
* use negative index to get a single character from the end of string like "string[-index]". "-1" is the last character.
* use colon to get substring by string[start-index : end-index]. The character at end-index is not included. 
    * If you don't specify start-index, then it starts from 0
    * If end-index is not specified, then it ends with the last character.
    * you can use negative index too like string[1, -1]. 
    
## Fomatting
* "+" - concatenation - suit for simple cases.
* f'string {var} string' - pre-formatted string. Curly braces are used to enclose variables.
* "*" - repetition - can repeat string several times like:
    ```python
    "string to be repeated" * 10
    ``` 
   
## Methods
All of the following methods do not change original string, they generate new strings.
* length() - number of characters of a tring. Or use the built-in function len() instead.
* upper() - to upper case. 
* lower() - to lower case. 
* find("substring-to-find") - find the index of the first occurrence of a substring.
    * this is similar to the "in" operator which returns True or False instread of index.
        ```python
        "substring" in "parent string"
        ```
* replace("substring-to-update", "new-substring"). 
* title() - converts first character of each word in a string to be upper case.


