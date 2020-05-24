# Bash Patterns and Regex

- Globs
- Extended globs
- Brace expansion
- Basic Regex
- Extended Regex

## Globs
Expands wild card chars into a list of un-quoted arguments  
Built into the shell  
Globbing support depends on shell type/version  

## Globs vs. Regex

- Globs match file names
- Regex match text
- Globs easier for system to process


Example:  
`ls [0-9]?file*.txt`

`[0-9]` - one digit in 0 thru 9  
`?` -     a single character  
`*` -     0 or more characters  

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

### Inconsistent Sorting
- Set LC_COLLATE=C  or C.UTF-8
- Set `globaasciiranges` shell option (Bash 4.3 or greater)
- bash -O globasciiranges
- Use character classes

### Character classes
Consistent and reliable  

- [:lower:]
- [:upper:]
- [:digit:]
- [:alpha:]
- [:alnum:]
- [:space:]
- [:graph:]     Printable chars not including spaces
- [:print:]     Printable chars including spaces
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

Example: globstar does a recursive search

    shopt -s globstar

    for i in **/*.txt ; do
      echo $i
    done


-----------------------------------
## Extended Globs 
-----------------------------------

- Match multiple occurrences
- Allow grouping patterns
- Nesting pattern groups
- Logical AND and OR

### Why Use Extended Globs
- Extended Globs execute faster than grep regex's
- Makes interactive globbing more useful
- Add power to `if` conditional statements
- Add power to `case` statements
- Add power to pattern substitution

### Examples
- @(pattern)  # match exactly one occurance
- @(pat1|pat2)  # match exactly one of pat1 or pat2
- ?(pattern)  # 0 or 1 occurance
- +(pattern)  # 1 or more occurances    *#
- *(pattern)  # 0 or more occurances
- !(pattern)  # invert the match
```
        !(+(photo|file)*+(.jpg|gif))
```

- ${var%.*}   # % stops at first match from the end
- ${var%%.*}   # % stops at furthest match from the end
- ${var%%@(.tar|.bak)*}   # % stops at furthest match from the end

```
    # file=Archive-2017-06-12.tar.gz
    # echo ${file}
    Archive-2017-06-12.tar.gz
    # echo ${file%.*}
    Archive-2017-06-12.tar
    # echo ${file%%.*}
    Archive-2017-06-12
    # echo ${file%%@(tar|bak)}
    Archive-2017-06-12
```

-----------------------------------
## Brace Expansion
-----------------------------------
- Globs expand pathnames
- Brace expansion expands braces
- Brace expansion is processed first
- Globs are processed last
- Brace expansion doesn't depend on existence of files
- Brace expansion operates on strings

Examples:
```
{1,3,5}
{1..5}
{1..4}{a..d}
{1..100..2}     // Step
{100..1..2}     // Step backwards
{002..100..2}   // pad w zeros
```
Use `{,.bak}` with `cp` or `mv`
```
cp /path/to/file.conf{,.bak}
```
```
for i in Backup*{0..30..7}.tar.bz2; do cp -v $i{,.bak}; done
```

-----------------------------------
## Regular Expressions
-----------------------------------
- BRE: Basic RegEx
- ERE: Extended RegEx
- PCRE: Perl Compatible RegEx

### regular-expressions.info/refflavors.html

## ERE: Extended RegEx
EREs are cleaner and easier to use  
 
    .       matches one char
    [ ]     character sets
    \       escape single char
    ( )     pattern grouping
    |       alternation
    # + {}  repetition operators
    ^ $     Beginning/Ending anchor
    [^asd]  negates char set


## BRE: Basic RegEx
Must escape some of the chars to give them their special meaning  

    \? \+ \{\} \| \(\)  
    
Just always use EREs  

### RegEx Support
grep -E or egrep:        ERE  
sed:            BRE  
sed -E:         ERE  
awk:            ERE  
bash [[ =~ ]]:  ERE

## Special Expressions w Backslashes
```
\b      empty string at word boundry
\B      empty string not at word boundry
\<      empty  string at beginning of word
\>      empty  string at end of word

\w same as [_[:alnum:]]     Matches whole word
\W same as [^_[:alnum:]]    Match non-word
\s same as [[:space:]]      Match a whitespace
\S same as [^[:space:]]     Match a non-whitespace

```

## Matching Occurrences
|Pattern | Match |
|--------|-------|
| .      | one char |
| .?     | zero or one char |
| .+     | one or more occurrence |
| .*     | zero or more occurrence |
| .{3}   | three occurrences |
| .{3,}  | three or more occurrences |
| .{,3}  | zero to three occurrences |
| .{1,3} | one to three occurrences |

## Alternation and Grouping
- Alternation allows matching one or another
- Grouping treats more than one pattern as one match

Examples:
```
dog|cat             dog or cat
^(dog|cat)?$        zero or one 'dog' or 'cat'
^(dog|cat)+$        one or more 'dog' or 'cat'
[0-9]|[1-9][0-9]    0 thru 99
[0-9][0-9]?         0 thru 99. zero or one of the second char set
^([0-9][0-9]?|1[0-4][0-9]|150)$     0 - 150 
```

## Back references
Only supported in BREs NOT EREs!
Max 9 back references
```
(ss).*\1            \1 references the first group - (ss)
^(.)(.).\2\1$       Match 5 letter palendone
```


