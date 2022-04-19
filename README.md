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
