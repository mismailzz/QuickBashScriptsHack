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
