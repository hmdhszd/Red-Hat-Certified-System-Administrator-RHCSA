
________________________________________________________________________________________________


# for

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


```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in $(cat names.txt);
do
        echo $x
done
```

________________________________________________________________________________________________



```bash
[bob@centos-host ~]$ nano loop-script.sh

#!/bin/bash

for x in {1..100};
do
        echo $x
done
```

________________________________________________________________________________________________




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


# while

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
