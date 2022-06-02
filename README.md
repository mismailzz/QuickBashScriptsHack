# quickBash
This repository contains the quick bash help. The snippets of bash scripts/code help to write the script fast. The snippets will further be added by the time, meanwhile if anyone want to contribute, we will be much happy about it. These are those snippets which I mostly use during writing the scripts.



<details>
<summary>Command Line Arguments with flags - Approach 1 <br>
 <b>[Example]~:</b> <i>./script.sh --domain  domain.com --expiry 1996,04,16 --dayslimitwarn 45 --dayslimitcri 30 --detail "my string info"</i>
 </summary>
<!--All you need is a blank line-->

     
	#!/bin/bash
	
	#Define the arguments list in the start of the file
	ARGUMENT_LIST=(
		"domain"
		"expiry"
		"dayslimitcri"
		"dayslimitwarn"
		"detail"
	)
    
	#Read arguments
	opts=$(getopt \
		--longoptions "$(printf "%s:," "${ARGUMENT_LIST[@]}")" \
		--name "$(basename "$0")" \
		--options "" \
		-- "$@"
	)

	
	eval set --$opts

	while [[ $# -gt 0 ]]; do
		case "$1" in
						
			--domain)
				DOMAIN=$2
				shift 2
				;;
	
			--expiry)
				EXPIRY=$2
				shift 2
				;;
	
			--dayslimitcri)
				DAYLIMITCRI=$2
				shift 2
				;;
	
			--dayslimitwarn)
				DAYLIMITWARN=$2
				shift 2
				;;
				
			--detail)
				DETAIL=$2
				shift 2
				;;
				
			*)
				break
				;;
		esac
	done
	
	#Now you can have the values
	echo $DOMAIN
	echo $EXPIRY
	echo $DAYLIMITCRI
	echo $DAYLIMITWARN
	echo $DETAIL
   
</details>


<details>
<summary>If-Statement with multiple condition check or comparison operator </summary>
<!--All you need is a blank line-->

	if [[ "$STR1" == "" ]] || [[ "$STR2" == "" ]] || [[ "$STR2" == "" ]];then
		echo "Arguments are not compelete"
		exit  
	fi
   
</details>


<details>
<summary>If-Else-Else_If Statement with multiple condition check or comparison operator </summary>
<!--All you need is a blank line-->

	if [[ "$STR1" == "" ]] || [[ "$STR2" == "" ]];then

		echo "Arguments are not compelete"
		exit  

	else

		if [[ "$STR1" -gt "$STR2" ]]
		then
			echo "String1: " $STR1 " String2: " $STR2
			exit
		elif [[( "$STR1" -gt "$STR2" )]] && [[( "$STR1" -le "$STR2" )]]
		then
			echo "String1: " $STR1 " String2: " $STR2
			exit 1
		elif [[( "$STR1" -le "$STR2" )]]
		then
			echo "String1: " $STR1 " String2: " $STR2
			exit 2

		fi

	fi
   
</details>

<details>
<summary>If-Statement short notes </summary>
<!--All you need is a blank line-->

```bash
#Ref: https://unix.stackexchange.com/questions/109625/shell-scripting-z-and-n-options-with-if
[ -z "$output2" ]  #string is null, that is, has zero length
[ -n "$output" ] #string is not null
[[ "$STR1" == "" ]]
[[ "$1" == "userdel" ]]
[[ "$STR1" -gt "$STR2" ]]
[[( "$STR1" -le "$STR2" )]]
[ ! -d "$dir" ] #dir does not exist
[ "$(echo "$dirperm" | cut -c6)" != "-" ] #Command execution in if statemnet
```
   
</details>


<details>
<summary>Run python command in bash script <br> 
<i>You can see that we are substituting or using variable of bash in this command. From python print we have the console output value and then we assign
that value to the bash variable for further tasks</i>
</summary>
<!--All you need is a blank line-->

	DAYS_LEFT=`python -c "from datetime import date; print((date($EXPIRY)-date.today()).days)"`
	echo $DAYS_LEFT
   
</details>


<details>
<summary>Function - Declaration and call a function in bash <br> 
<i>I use to define the function in the top of the script and if i have the command lines arguments then I declare the function after it. So we can call the function anywhere in the script</i>
</summary>
<!--All you need is a blank line-->

	#Function definition
	help_function(){
	
	echo "-----HELP FUNCTION-------"
	echo "script.sh [OPTIONS BELOW]"
	
	echo "PARAMETERS"
	echo ""
	echo "--domain <Domain Name>"
	echo "--dayslimitcri <Days Limit for Critical>"
	echo "--dayslimitwarn <Days Limit for Warning>"
	echo "--detail <Other detail>"
	echo ""
	
	echo "Info: At this moment all parameters should be present for execution of script"
	echo "-------------------------"
	}
	
	#Function call
	help_function
   
</details>

	
<details>
<summary>Loop - Read file line by line <br> 
</summary>
<!--All you need is a blank line-->

	while read line; do
	  echo "$line"
	done <file.txt
   
</details>	
	
<details>
<summary>Loop - Simple (Iterate over a list) <br> 
</summary>
<!--All you need is a blank line-->

	for i in element1 element2;
	do 

	echo $i

	done
   
</details>

<details>
<summary>Get the files/folders from current dir having size in GB<br> 
</summary>
<!--All you need is a blank line-->
	
	sudo du -sh * | awk '($1~/[0-9]+\.?[0-9]*G$/)' | cut -f2 | (while read -r dir_files; do
		find ./$dir_files -prune -exec stat --printf='User: %U | Group: %G | Size: ' {} \; -exec du -sh {} \; 
	done)   

</details>

<details>
<summary>How to execute a script in execution of any command in linux? like if a user runs "id" command then after that your script would run</summary>
<!--All you need is a blank line-->

```bash
#Define/Declare below function in /etc/bashrc 
#~ confirm anycommand
#Ref: https://askubuntu.com/questions/22233/always-prompt-the-user-before-executing-a-command-in-the-shell
#We can show the message on the binaries in this way
confirm() {
    echo -n "Do you want to run $*? [N/y] "
    read -N 1 REPLY
    echo
    if test "$REPLY" = "y" -o "$REPLY" = "Y"; then
		"$@"
	else
        echo "Cancelled by user"
    fi
}
```
   
</details>
	

<details>
<summary>Print message on execution of userdel command in linux and do'nt allow the deletion of user</summary>
<!--All you need is a blank line-->

```bash
#Define/Declare below function in /etc/bashrc 
#~ confirm userdel user_name
#We can show the message on the binaries in this way
confirm() {
    echo -n "Do you want to run $*? [N/y] "
    read -N 1 REPLY
    echo
    if test "$REPLY" = "y" -o "$REPLY" = "Y"; then
        if [[ "$1" == "userdel" ]]; then
          echo "Contact to Admin"
        fi
    else
        echo "Cancelled by user"
    fi
}
```
   
</details>
