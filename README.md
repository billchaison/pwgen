# pwgen
Password generation script in bash

Allows you to specify complexity requirements for the string creation

```bash
#!/bin/bash

if [ $# -ne 5 ]
then
	echo "You must supply 5 values:"
	echo "   The password length in characters"
	echo "   The minimum number of lower case letters"
	echo "   The minimum number of upper case letters"
	echo "   The minimum number of digits"
	echo "   The minimum number of special symbols"
	exit 1
fi

if [ $1 -lt $(($2 + $3 + $4 + $5)) ]
then
	echo "The specified password length is too short"
	exit 1
fi

nlc=$2
nuc=$3
ndg=$4
nsy=$5

dif=$(($1 - $(($2 + $3 + $4 + $5))))

LC=(61 62 63 64 65 66 67 68 69 6a 6b 6c 6d 6e 6f 70 71 72 73 74 75 76 77 78 79 7a)
UC=(41 42 43 44 45 46 47 48 49 4a 4b 4c 4d 4e 4f 50 51 52 53 54 55 56 57 58 59 5a)
DG=(30 31 32 33 34 35 36 37 38 39)
SY=(2d 3d 2b 2e 2a 25 24 23 40 21 3f 3a)

PW=()

while [ $nlc -gt 0 ]
do
	PW+=(${LC[$RANDOM % ${#LC[@]}]})
	nlc=$(($nlc - 1))
done
while [ $nuc -gt 0 ]
do
	PW+=(${UC[$RANDOM % ${#UC[@]}]})
	nuc=$(($nuc - 1))
done
while [ $ndg -gt 0 ]
do
	PW+=(${DG[$RANDOM % ${#DG[@]}]})
	ndg=$(($ndg - 1))
done
while [ $nsy -gt 0 ]
do
	PW+=(${SY[$RANDOM % ${#SY[@]}]})
	nsy=$(($nsy - 1))
done
while [ $dif -gt 0 ]
do
	r=$((RANDOM % 4))
	if [ $r -eq 0 ]
	then
		PW+=(${LC[$RANDOM % ${#LC[@]}]})
	fi
	if [ $r -eq 1 ]
	then
		PW+=(${UC[$RANDOM % ${#UC[@]}]})
	fi
	if [ $r -eq 2 ]
	then
		PW+=(${DG[$RANDOM % ${#DG[@]}]})
	fi
	if [ $r -eq 3 ]
	then
		PW+=(${SY[$RANDOM % ${#SY[@]}]})
	fi
	dif=$(($dif - 1))
done

while [ ${#PW[@]} -gt 0 ]
do
	AE=$(($RANDOM % ${#PW[@]}))
	SPW=$SPW${PW[$AE]}
	unset PW[$AE]
	PW=( "${PW[@]}" )
done
NL="0a"

echo $SPW$NL | xxd -r -p
```
