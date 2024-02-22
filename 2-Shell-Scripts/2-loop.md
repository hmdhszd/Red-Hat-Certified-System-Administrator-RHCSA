
# `for`

```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in first second third;
do
        echo $x
done
```



```bash
[bob@centos-host ~]$ chmod +x loop-script.sh 

[bob@centos-host ~]$ ./loop-script.sh 
first
second
third
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in `cat names.txt`;
do
        echo $x
done
```

________________________________________________________________________________________________

## `$(cat names.txt)`

```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in $(cat names.txt);
do
        echo $x
done
```

________________________________________________________________________________________________


## `{1..100}`

```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in {1..100};
do
        echo $x
done
```

________________________________________________________________________________________________


## `(( x = 0 ; x <= 10 ; x++ ))`

```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for (( x = 0 ; x <= 10 ; x++ ))
do
        echo $x
done
```

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for FILE in $(ls)
do
        echo Line count of $FILE is $(cat $FILE | wc -l)
done
```

________________________________________________________________________________________________


# `while`

x=$(( $x + 1 ))

```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash
x=1
while [ $x -le 5 ]
do
  echo "Welcome $x times"
  x=$(( $x + 1 ))
done
```

________________________________________________________________________________________________


((i++))

```bash
#!/bin/bash

i=0

while [[ $i -lt 11 ]] 
do
        if [[ "$i" == '2' ]]
        then
                echo "Number $i!"
                break
        else
                continue
        fi
        echo $i
        ((i++))
done

echo "Done!"
```


________________________________________________________________________________________________


### Command-line arguments 

- `$0` --> Script Name

- `$1` --> First Argument

- `$2` --> Second Argument

- `$@` --> List of Arguments

- `$#` --> Total Number of Arguments

- `$$` --> Process ID

- `$?` --> Exit Code of the script

________________________________________________________________________________________________


```bash
[bob@centos-host ~]$ cat my-script.sh

#!/usr/bin/bash

echo "Script Name: ---->  $0"
echo "First Argument of the script is: ---->  $1"
echo "The second Argument is: ---->  $2"
echo "The complete list of Arguments is: ---->  $@"
echo "Total Number of Arguments: ---->  $#"
echo "The Process ID is: ---->  $$"
echo "Exit Code of the script: ---->  $?"
```

```bash
[bob@centos-host ~]$ ./my-script.sh This_is_my_first_arg This_is_my_second_arg

Script Name: ---->  ./my-script.sh
First Argument of the script is: ---->  This_is_my_first_arg
The second Argument is: ---->  This_is_my_second_arg
The complete list of Arguments is: ---->  This_is_my_first_arg This_is_my_second_arg
Total Number of Arguments: ---->  2
The Process ID is: ---->  1547
Exit Code of the script: ---->  0
[bob@centos-host ~]$ cat my-script.sh 
```


________________________________________________________________________________________________


Create a script named "/find.sh" that counts the number of regular files matching a specified pattern (provided as the first argument) within the "/home" directory and its subdirectories.



```bash
touch /find.sh

chmod +x /find.sh
```


```bash
#!/bin/bash

if [[ -z "$1" ]];
        then
                echo "Error: Please provide a pattern to search for. ($1 is null)"
                exit 1
fi




count=$(find /home -type f -name "$1" 2>/dev/null | wc -l) || {

echo "Error: An issue occurred during the file search."

exit 1

}

echo "Found $count matching files in /home and its subdirectories."
```




________________________________________________________________________________________________





