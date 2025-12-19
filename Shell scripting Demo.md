# variable declartion

```sh
PRICE_PER_APPLE=5
MYFirstLetters=ABC
greeting="Hello world!"

echo "The Price of an apple today is: \$HK $PRICE_PER_APPLE"
echo "The first letter :${MYFirstLetters}"

greeting="Hello world"
echo "greeting now with spaces :$greeting"

FILELIST=$(ls)
echo "Files in current directory:"
echo "$FILELIST"

FilewithTimeStamp="/tmp/my-dir/file_$(date +%y-%m-%d).txt"
echo "Generated file name: $FilewithTimeStamp"

```

# Example
vi my_cart.sh
```sh
echo "File name is "$0 # holds the current script
echo $3 # $3 holds banana
Data=$5
echo "A $Data costs just $6."
echo $#
```

`sh mycart.sh 10 30 50`

# Read input from the user
```sh
read -p "Enter the fruit name to purchase: " fruit

price=60
echo "Chosen Fruit is $fruit, cost per item is ₹$price"

read -p "Enter the quantity you want: " quantity

total=$((price * quantity))

echo "Total Bill is ₹$total"

```
```sh
echo "Enter the file name"
read file
#file="report.txt"

if [ -f "$file" ]
then
    echo "File exists"
else
    echo "File not found"
fi

```

# example for if else


```sh
echo "Enter your Salary"
read salary
echo "Enter the Designation (Manager /Employee)"
read designation
if [ "$salary" -ge 50000 ] && [ "$designation" = "Manager" ]
then
        echo "Your are a senior manager"
elif [ "$salary" -ge 30000 ] && [ "$designation" = "Employee" ]
then
        echo "You are a permenant employee"
else
        echo "You are an associate or Junior Employee"
fi

```

# Example for While loop
```sh
usage=90
while  [ $usage -gt 80 ]
do
        echo "Disk usage is High ${usage}%"
        break
done

```


# Max attempts validation
```sh
#!/bin/sh

CORRECT_PASSWORD="admin@123"
attempts=0
MAX_ATTEMPTS=3

while [ $attempts -lt $MAX_ATTEMPTS ]
do
    echo "Enter your password:"
    read pass

    if [ "$pass" = "$CORRECT_PASSWORD" ]
    then
        echo "✅ Login successful! Welcome to the system."
        exit 0
    else
        attempts=$((attempts + 1))
        remaining=$((MAX_ATTEMPTS - attempts))
        echo "❌ Incorrect password."

        if [ $remaining -gt 0 ]
        then
            echo "⚠️ You have $remaining attempt(s) left."
        fi
    fi
done

echo "🚫 Account locked! Too many failed login attempts."
exit 1
```

# Select  with case example
```sh
select option in Start Stop Restart Exit
do
    case $option in
        Start) echo "Starting service" ;;
        Stop) echo "Stopping service" ;;
        Restart) echo "Restarting service" ;;
        Exit) break ;;
        *) echo "Invalid choice" ;;
    esac
done
```

you need to use bash command where as sh does not support


```sh
cp ~/projects/python/backup/sample.txt /home/admin1/sample_bak.txt
if [ $? -ne 0 ]; then
    echo "Backup failed"
    exit 1
fi
echo "Backup completed"
```

```sh

LOGFILE="$HOME/projects/python/log1.txt"
if [ ! -d "$HOME/projects/python" ]; then
    echo "❌ Directory does not exist"
    exit 1
fi 
echo "Writing log..." 
 echo "Application started at $(date)" >>"$LOGFILE"  
  if [ $? -ne 0 ]; then 
   echo "❌ Failed to write to log file"
    exit 2  fi 
 echo "✅ Log written successfully"     
```

```sh
check_even(){
        if [ $(( $1 % 2 -eq 0 ];then
            return 0
        else
                return 1
fi 
check_even 10
echo $?
```