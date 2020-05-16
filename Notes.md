# Bash Patterns and Regex

- Globs
- Extended globs
- Brace expansion
- Basick Regex
- Extended Regex

## Globs
Expands wild card chars into a list of un-quoted arguments  
Built into the shell  
Globbing support depends on shell type/version  

## Globs vs. Regex
- Globs match file names
- Regex match text
- Globs easier for system to process  

    `ls [0-9]?file*.txt`

[0-9]     one digit in 0 thru 9
?         a single character
*         0 or more characters

The expansion is done by the shell. Then the list is given to the `ls` command  

Here is similar command with regex  
    `ls | grep '[0-9].file.*\.txt'`

Use 'single quotes' to keep the shell from interpreting a regex as a glob  
    `grep '^A.*\.txt'  *.txt`


### Types of Shell Expansion and Order of Precedence

- Brace
- Tilde 
- Parameter 
- variable
- Command Substitution and Arithmetic (processed from left to right)
- Word Splitting
- Filename 
- Quote removal

## Expansions that can change the number of words
- Brace:  {1..10)  -> 1 2 3 4 ...
- word splitting 
- File expansion:  *.txt -> notes.txt sn.txt version.txt  

## Expands to a single word
- $((2 + 2))
- $(whoami)  # ?? not sure about this 
- $USER

## Glob chars
### Wild Cards
- `*`
- `?` 
### Character sets
- [123abc]
- [1-8a-g]
- [^asdf] or [!asdf]  #inverts the set, meaning, not these chars
- [\^]
- [\!]
- [asd-]
- [[as] or []asdf]    #a left or right bracket must be first in the list to loose its meaning

### Character classes
- [:lower:]
- [:upper:]
- [:digit:]
- [:alpha:]
- [:alnum:]
- [:space:]
- [:punct:]
- [:cntrl:]
- [:xdigit:]    Hexadecimal characters

Examples

    [[:lower:]]
    [[:alpha:][:space:]]
    [![:alpha:][:space:]]


## Shell Glob Options
    shopt -s        # set option  
    shopt -u        # unset option  
    shopt -p        # print options  

- globstar          # recursive search
- nocasematch       # case insensitive
- extglobs          # extended globs

Example: globstart does a recursive search

    shopt -s globstar

    for i in **/* ; do
      echo $i
    done


-----------------------------------
## Extended Globs 
-----------------------------------
Extended Globs execute faster than grep regex's

- @(pattern)  # match exactly one occurance
- @(pat1|pat2)  # match exactly one of pat1 or pat2
- ?(pattern)  # 0 or 1 occurance
- +(pattern)  # 1 or more occurances    *#
- *(pattern)  # 1 or more occurances
- !(pattern)  # invert the match

    !(+(photo|file)*+(.jpg|gif))

