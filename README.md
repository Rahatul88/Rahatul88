- 👋 Hi, I’m @Rahatul88
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Rahatul88/Rahatul88 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->#!/bin/bash

#########
# COLORS
#########

#colors
#colors
white="\033[1;37m"                                          ##
grey="\033[0;37m"                                           ##
purple="\033[1;35m"                                         ##
red="\033[1;31m"                                            ##
green="\033[1;32m"                                          ##
yellow="\033[1;33m"                                         ##
purple="\033[0;35m"                                         ##
cyan="\033[0;36m"                                           ##
cyan1="\033[1;36m"                                          ##
cafe="\033[0;33m"                                           ##
fiuscha="\033[0;35m"                                        ##
blue="\033[1;34m"                                           ##
l_red="\033[1;37;41m"                                       ##
nc="\033[0m"                                                ##

arch=`uname -m`

#spinner
spinner() {
        pid=$!
        spin='\|/-'
        i=0
        tput civis
        while kill -0 $pid 2>/dev/null
	do
                i=$(( (i+1) %4 ))
                printf "\r${cyan1}[${spin:$i:1}]${nc} ${cyan1} $launch"
                sleep .1
	done
        printf "\r   ${green}[✔]${nc} ${green} $splashdown";echo
        tput cnorm
}

function os () {
	cat /etc/os-release > /dev/null 2>&1
	if [ "$?" -eq "0" ]; then
		OS=DEBIAN
		BIN="/usr/bin"
		main="/usr/share/shark"
	else
		OS=TERMUX
		BIN="${PREFIX}/bin"
		main="${PREFIX}/share/shark"
		pkg update -y > /dev/null 2>&1
		pkg install ncurses-utils -y > /dev/null 2>&1
	fi
}

function git_clone() {
	launch="cloning shark";splashdown="shark cloned";echo
	[ -d "${main}" ] && rm -rf ${main} > /dev/null 2>&1
	(git clone https://github.com/bhikandeshmukh/shark --quiet ${main}) & spinner
}


function dependencies() {
        launch="installing lib";splashdown="installed lib"; echo
    		if [ ${OS} != "DEBIAN" ]; then
    			(pkg install apache2 php jq curl wget git -y > /dev/null 2>&1) & spinner
		else
			(sudo apt-get install ${i} -y > /dev/null 2>&1) & spinner
		fi
}


function setup_ngrok() {
	if [ ! -f  "${BIN}/ngrok" ]; then
		if [[ ("$arch" == *'arm'*) || ("$arch" == *'Android'*) ]]; then
			rm -rf $BIN/ngrok
			ngrok_url="https://github.com/Bhaviktutorials/Ngrok_magical/raw/main/Magicalrok.zip"
		elif [[ "$arch" == *'aarch64'* ]]; then
			rm -rf $BIN/ngrok
			ngrok_url="https://github.com/Bhaviktutorials/Ngrok_magical/raw/main/Magicalrok.zip"
		elif [[ "$arch" == *'x86_64'* ]]; then
			ngrok_url="https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip"
		else
			ngrok_url="https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-386.zip"
		fi
		launch="installing ngrok";splashdown="installed ngrok";echo
		(wget --quiet ${ngrok_url} -O ngrok.zip) & spinner
		unzip -q ngrok.zip && rm -rf ngrok.zip && chmod +x ngrok
		mv ngrok $BIN
	fi
}

function final_touchup() {
		if [ ${OS} != "DEBIAN" ]; then
		printf "\n$yellow Enter your Ngrok Authtoken to continue [Ex. ngrok authtoken I7QUQ ] : $nc"
		read ngrok_authtoken
		$ngrok_authtoken > /dev/null 2&>1
	        fi
		for i in shark; do
			chmod +x ${main}/${i}
			mv ${main}/${i} ${BIN}
			printf "\n${green}>>${white} simply run : shark \n\n ${nc}"
		done
}

os
dependencies
git_clone
setup_ngrok
final_touchup

#####################################
#       DON'T TRY TO COPY 😂😂     #
#   JUST LEARNING KEEP SUPPORTING   #
#             THANKYOU              #
#####################################
