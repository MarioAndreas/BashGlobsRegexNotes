# Questions

## Q: What is Word Splitting?  
The shell scans the results of parameter expansion, command substitution, and arithmetic expansion that did not occur within double quotes for word splitting.  
If double quotes are used, then only a single string is returned  

Examples:  
Without double quotes `Word Splitting` IS performed on the expansion 
```bash
$ ls $(uname -a)
ls: cannot access 'Linux': No such file or directory
ls: cannot access 'Mobile-PC': No such file or directory
ls: cannot access '4.4.0-18362-Microsoft': No such file or directory
ls: cannot access '#836-Microsoft': No such file or directory
ls: cannot access 'Mon': No such file or directory
ls: cannot access 'May': No such file or directory
ls: cannot access '05': No such file or directory
ls: cannot access '16:04:00': No such file or directory
ls: cannot access 'PST': No such file or directory
ls: cannot access '2020': No such file or directory
ls: cannot access 'x86_64': No such file or directory
ls: cannot access 'x86_64': No such file or directory
ls: cannot access 'x86_64': No such file or directory
ls: cannot access 'GNU/Linux': No such file or directory
```
Wiith double quotes `Word Splitting` does NOT occur on the expansion.
```bash
$ ls "$(uname -a)"
ls: cannot access 'Linux Mobile-PC 4.4.0-18362-Microsoft #836-Microsoft Mon May 05 16:04:00 PST 2020 x86_64 x86_64 x86_64 GNU/Linux': No such file or directory
```
With single quotes NO `expansion` occurs
```
$ ls '$(uname -a)'
ls: cannot access '$(uname -a)': No such file or directory
```

## Q: What is Parameter expansion?
Parameters passed into a script:  $0 $1 $@