MAIN --->> https://github.com/LondheShubham153/Shell-Scripting-For-DevOps/tree/main

1. Shell scripting is linux based 
2. Shell scripting means writing shell commands in a sequence in a file
3. Where is our shell in linux /bin/bash
4. Extension is .sh , can do which bash command to know where bash is 
5. THE FIRST LINE IN SHELL SCRIPT is called as shaban, here we have to tell what type of shell it is we are using EG: #!/bin/bash “Through this line interpreter knows on what shell to run it”.
6. Error Handling is a must in shell scripts using if else
7. functions are also important in shell scripting 

https://www.youtube.com/watch?v=9Xl1ZTk3BQw&t=3122s

## Comments:

we use # for **single line** comment

Multi Line: 

```jsx
<< AnyNameORWord  

this comes under comment

AnyNameORWord  

```

## BASH FILE PRACTICE 1:
```
#!/bin/bash 

read -p "Enter Your Favorite Sahabi Name: " sahabiName

if [[ $sahabiName == "Omer" ]];
then 
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "Abu Bakar" ]]; 
then 
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "Osman" ]];
then
       echo "Ya Salam  $sahabiName"

elif [[ $sahabiName == "Umer" ]];
then
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "AbuBakar" ]];
then
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "Usman" ]];
then
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "Khalid bin Waleed" ]];
then
       echo "Ya Salam $sahabiName "

elif [[ $sahabiName == "Abdur Rehman Bin Auf" ]];
then
       echo "Ya Salam $sahabiName"       

elif [[ $sahabiName == "AbdurRehman Bin Auf" ]];
then
       echo "Ya Salam $sahabiName "

else 
       echo "Ya Ali Madad"
fi      

```


## BASH FILE PRACTICE 2:
```
#!/bin/bash
 

for (( i=0; i < $1; i++   ));
do 
	rm -r "day$i"

done
```

## BASH FILE PRACTICE 3:
```
#!/bin/bash


num=1

while [[ $num -le  5 ]] 
do 
	echo "h1$num"
	num=$((num+1))

done


```

## BASH FILE PRACTICE 4:

```
#!/bin/bash
 
function name(){ 
	echo "Hi $1 "
}	

name "omer"


```

## BASH FILE PRACTICE 5:
```
# Practice Scripts — Zero to Hero Shell Scripting

A numbered, beginner-friendly progression through shell scripting. Follow the scripts in order from `01` to `18` — each one builds on the previous.

Run any script with:

```bash
bash 01_hello.sh
# or make it executable first
chmod +x 01_hello.sh && ./01_hello.sh
```

## The sequence

### Phase 1 — Output, variables, input

| # | Script | What you'll learn |
|---|--------|-------------------|
| 01 | `01_hello.sh` | Shebang `#!/bin/bash`, `echo`, comments |
| 02 | `02_variables.sh` | Variable assignment, expansion, `$(command)` substitution |
| 03 | `03_arguments.sh` | Script args: `$0`, `$1`, `$#`, `$@` |
| 04 | `04_user_input.sh` | Interactive input with `read -p` |

### Phase 2 — File operations & operators

| # | Script | What you'll learn |
|---|--------|-------------------|
| 05 | `05_create_file.sh` | `touch`, chaining with `&&` and `\|\|` |
| 06 | `06_install_package.sh` | Parameterized `sudo apt-get install` |

### Phase 3 — Conditionals

| # | Script | What you'll learn |
|---|--------|-------------------|
| 07 | `07_if_else.sh` | `if [[ ]] … else … fi`, string comparison `==` |
| 08 | `08_if_file_exists.sh` | File test `-f`, arg count `$#`, `exit 1` |
| 09 | `09_if_elif_else.sh` | Multi-branch with numeric `-ge` and string checks |

### Phase 4 — Loops

| # | Script | What you'll learn |
|---|--------|-------------------|
| 10 | `10_for_loop_list.sh` | `for x in list` and glob patterns |
| 11 | `11_for_loop_counter.sh` | C-style `for (( ))` to create N folders |
| 12 | `12_while_loop.sh` | `while [[ ]]` with arithmetic `$((...))` |

### Phase 5 — Functions

| # | Script | What you'll learn |
|---|--------|-------------------|
| 13 | `13_functions.sh` | Define and call a function |
| 14 | `14_function_with_args.sh` | Functions with parameters (`$1` inside function) |
| 15 | `15_source_and_call.sh` | `source` another script and reuse its functions |

### Phase 6 — Real DevOps tasks

| # | Script | What you'll learn |
|---|--------|-------------------|
| 16 | `16_create_user.sh` | Full workflow: input → check with `id` → `useradd` |
| 17 | `17_system_details.sh` | Pipes + `awk` for memory, disk, and process info |
| 18 | `18_error_handling.sh` | `set -e`, exit codes, `if ! func; then exit 1` |

### Phase 7 — Capstone: backups + cron

| # | Script | What you'll learn |
|---|--------|-------------------|
| 19 | `19_backup.sh` | `tar -czf` gzip archive, timestamped filenames, retention cleanup |

`19_backup.sh` applies everything from 01–18: variables, `$(date)`, functions, `if`-checks, `set -e`, conditionals. It creates a timestamped `.tar.gz` of `practice-scripts/` and keeps only the last 7 backups.

**Run manually:**

```bash
bash 19_backup.sh
# backups go to ~/shell-backups/ by default
# override with: BACKUP_DIR=/my/path bash 19_backup.sh
```

**Schedule with cron (periodic backups):**

Open your crontab:

```bash
crontab -e
```

Add one of these lines (replace the path with your actual repo path):

```cron
# every day at 2 AM
0 2 * * * /bin/bash /Users/shubhamlondhe/Documents/work/TrainWithShubham/courses/udaan-batch-11/Shell-Scripting-For-DevOps/practice-scripts/19_backup.sh >> /tmp/backup.log 2>&1

# every hour
0 * * * * /bin/bash /path/to/practice-scripts/19_backup.sh >> /tmp/backup.log 2>&1

# every 15 minutes (testing only)
*/15 * * * * /bin/bash /path/to/practice-scripts/19_backup.sh >> /tmp/backup.log 2>&1
```

Cron field order: `minute hour day-of-month month day-of-week command`. Use [crontab.guru](https://crontab.guru) to build/test expressions.

List your scheduled jobs: `crontab -l`. Remove all: `crontab -r`.

## What's next? (Cloud)

You've finished zero-to-hero basics. For real-world cloud automation, study these advanced scripts in the parent repo (they apply everything above plus AWS CLI and Docker):

- `../day03/deploy_django_app.sh` — multi-step Docker deployment from a Git repo
- `../day03/create_ec2.sh` — provision an AWS EC2 instance end-to-end with `set -euo pipefail` and polling loops

Note: those need an AWS account and `aws-cli` configured.
```
