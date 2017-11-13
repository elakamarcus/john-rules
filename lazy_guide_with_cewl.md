# Lazy pwd guessing
## Prerequisit
- Kali 2.0 VM image, download from offensive security.
- Internet connection
    - if no, do the actions ``` wget ``` and ``` cewl ``` in advance and have the files ready in the VM...

## Preparation
This is done once per Linux instance. If you reset the VM or run on another machine, repeat this step.

```zsh
wget http://contest-2010.korelogic.com/rules.txt
cat rules >> /etc/john/john.conf
```
preferably in project-specific folder

```zsh
cewl -d 2 -a --meta_file testmeta -e --email_file testemail -w cwords.l {URLGOESHERE}
```
### Explanation
Parameters are:

- ```-d 2``` the depth of the website, how far to follow links.
- ```- a --meta_file afile``` include metadata and save it to afile.
- ```-e --email_file efile``` include email findings and save to efile.
- ```-w cwords.l``` output the scraped words to cwords.l


## Execute

This step will initiate the password guessing.

```zsh
for i in `grep KoreLogic /etc/john/john.conf | cut -d":" -f2 | cut -d"]" -f1| tail -n60`; do john hash --wordlist=cwords.l --rules=$i; done
```

### Explanations
There are a number of programs executing here. So let's go through one by one:

#### for-loop
the for-loop will execute a command, and from each result do an action.

- ```for i in ``` this defines the variable to be utilised, $i.
- ``` `command` ``` this is the command to execute, note the character used to encapsulate the command.
- ``` do ... done ``` this is what the for-loop will do with the result of the command.

#### cut
cut is used to 'cut' out fields or columns. 

- ``` -d":" ``` this defines the delimeter for the field, in this case ":"
- ``` -f1 ``` specify one or several fields to extract. in this case the first.

#### john
this is the program that will do the password guessing.

- ``` john ``` execute the program ~~
- ``` hash ``` a file containing password hashes, one for each line.
- ``` --wordlist=cwords.l ``` use the wordlist called cwords.l, or provide exact path:
    - ``` --wordlist=/root/passwordlist/cwords.l ``` like this
- ``` rules=rule ``` tell john to use one of the defined rules, to manipulate the words of the wordlist. e.g. if the wordlist has "password" and the rule is to "appendnum", this will make a virtual list of ```password0, password1,...,password9```.

To see any progress, you can tail the pot-file.
```zsh
tail -f .john/john.pot
```

Enjoy!
