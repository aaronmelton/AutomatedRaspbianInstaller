# Automated Raspbian Installer #
---

2014-01-10: It appears that due to inactivity from the original author, the
rpi_installer_08-19-12.zip installer image (which this script was created for)
is no longer endorsed by the Raspberry Pi Foundation.  Please look at [hifi's raspbian-ua-netinst] (https://github.com/hifi/raspbian-ua-netinst) 
for instructions on how to create an unattended installation with a more recent installer image.

## About ##
Automated Raspbian Installer uses a custom cmdline.txt and preseed.cfg file 
to create an unattended, automated Raspberry Pi install using the Raspbian 
Installer image.

Installing a pre-compiled image onto an SD Card is nice, but I'm a control 
freak and like to know what actually goes on "behind the scenes" during an 
install.  I want the ability to include as much or as little as I want at 
the time of installation.  And I want to automate the entire process so I 
don't have to keep watch over the terminal to answer questions I know I can 
provide the answer to before the question is asked.

Therefore, I've painstakingly run dozens of network installations of Raspbian 
in order to create these files that allow you to preseed your next Raspbian 
installation. :)

## Dependencies ##
These files were built with the Raspbian Installer in mind.  They may work for 
other Debian-based network installations on a Raspberry Pi, but that's outside 
the scope of this project.

[Download the (old) Raspbian Installer image](http://archive.raspbian.org/installer/rpi_installer_08-19-12.zip), rpi_installer_08-19-12.zip, and follow their instructions for copying these files onto your SD Card.

## Requirements ##
1. Both files are required to complete a fully automated installation.
2. The preseed.cfg file must be accessible via http protocol, either on your 
local network or a publicly-accessible web server.
3. Edit the cmdline.txt file to point to the location of your preseed.cfg file:

	    preseed/url=http://host/path/to/preseed.cfg
4. Edit any other values in the cmdline.txt or preseed.cfg file to suit your 
own needs.  The comments in either file should be sufficient to help guide you.


## Assumptions ##
1. You assume all responsibility for anything that happens using these files.
2. You're welcome to use the default values I provide in these files or change 
them to suit your own needs.

## Limitations ##
1. Based on my testing, you can only pass approximately 350 characters worth 
of information to the installer through the cmdline.txt file (not including 
comments).  Any number or combination of commands can be used so long as they 
do not exceed this length in the file.  Anything beyond that length will not 
be read by the installer.  Keep this in mind because if the URL of your 
preseed.cfg file is too long, the installer may not be able to fetch it.
2. The preseed.cfg file must exist on a web server that is reachable via HTTP 
protocol.  (I suppose ftp may work but it was not tested.)  HTTPS **will not 
work**.  That means that you won't be able to fetch this file directly from 
GitHub, but have to host it on your own.  I originally tried to download this 
(raw) file directly from the GitHub repository but this must be a limitation 
of network installs.  If you know of a creative way to overcome this, please 
let me know! 
3. As the preseed.cfg is currently configured, this is a minimal installation 
of Raspbian.  In other words, you get SSH access and that's about it.  If you 
want a Desktop (LXDE, XFCE4, etc), you'll have to install those packages from 
the command line.  Unless, of course, you edit preseed.cfg to install them for 
you.  (Which might not be a bad idea if you're installing unattended anyway.)

## Functionality ##
1. The cmdline.txt file contains enough data to answer the initial installer 
questions to get you through the country, language and keyboard layouts.  
2. It then extends the DHCP delay time to 60 seconds.  My testing showed that 
the Raspberry Pi would obtain a DHCP lease from my server if permitted enough 
time to do so.  The default value (20 seconds?) is not enough.
3. Finally, the cmdline.txt file points to the location of the preseed.cfg 
file to provide answers to the rest of the questions encountered by the installer.
4. The values in the cmdline.txt and preseed.cfg file are specific to me.  
Please change them to meet your own installation needs.  

**For the security-conscious**: My Raspberry Pi is behind a firewall and I 
change the root password immediately upon logging in for the first time.  I 
also create my normal user account immediately after that.  I wouldn't suggest 
you operate as root OR run around with a root/root login to any box. You've 
been warned. :)
