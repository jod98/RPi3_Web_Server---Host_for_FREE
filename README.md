# RPi3_Web_Server (Self Hosted Public Website)

![GitHub - Website](https://user-images.githubusercontent.com/36043248/87041048-8c15c880-c1e9-11ea-8927-57134afa0477.PNG)

--------------------------------------------------------------------------------------------------------------------------------

1. Download Raspian Lite @ https://www.raspberrypi.org/downloads/raspberry-pi-os/
2. Flash the “.zip” file onto your SD card (preferably 8GB or greater) using Etcher @ https://www.balena.io/etcher/
3. Insert SD card into Raspberry Pi and connect it to your router via Ethernet cable.
4. Login to Pi using 
>Username: “*pi*” & 
>Password: “*raspberry*”
5. Run “*sudo raspi-config*” to enable SSH so we can login using our remote machine.
6. Go to “Interfacing Options” and then enable SSH.
7. Login into your Pi using your computer by simply typing in the IP address of the Pi. Type “hostname -l” into the Pi to retrieve IP address if unsure.
8. Download Putty @ https://www.putty.org/ and enter in the IP address of the Pi to access it remotely via the SSH protocol.
9. Follow the steps listed in this tutorial to configure the web server setup i.e. hosting your own website. https://projects.raspberrypi.org/en/projects/lamp-web-server-with-wordpress
10. Throughout this tutorial you will find commands like “*sudo leafpad*”. which is only capable of running with the “Desktop” version of the Raspian OS (GUI). With this lightweight OS version minimising resource usage you can replace “*sudo leafpad*” with “*sudo nano*” to edit the files via the Pi terminal.
11. On the “Install SQL” page, replace the command “*sudo apt-get install mysql-server php-mysql -y*” with “*sudo apt install mariadb-server mariadb-client php-mysql -y*”. In a Recently, Raspbian MySQL has been replaced replaced with MariaDB, a fork of the MySQL. 
12. Continue with the guide @ https://projects.raspberrypi.org/en/projects/lamp-web-server-with-wordpress
13. VOILA! You have your own Raspberry Pi Web Server perfectly capable of Hosting Your Own Website. This solution is **WAY CHEAPER** than purchasing Web Hosting from vendors like “BlueHost” and “HostGator”. You also have the comfort, leisure and privacy to keep your **PERSONAL DATA**. 

Note: This process will allow you to self host and edit your own personal website locally. To make your website available for public consumption, we will perform the following...

