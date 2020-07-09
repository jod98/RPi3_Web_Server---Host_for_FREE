# RPi3_Web_Server (Host Your Website for FREE)

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
13. After this process is completed, retrieve your public IP address of your Pi as we will need this later... Simply type in “*curl ifconfig.me*” and write down this address. Should be of the form "xxx.xx.xx.xxx". 

Note: This process will allow you to create and host your own website **LOCALLY**. To make your website **PUBLIC** i.e. accessible by anyone and not just within your own network then complete the following...

14. We want to forward a port on our router using "Port Forwarding". We can access this section by logging into our router usually available @ "192.168.1.1" and navigating to the "Port Forwarding" or "Port Mapping" section. We simply want to forward the port of Raspbery Pi's private IP address. Enter "80" for both the "External port" and "Internal port" as this is the port dedictated to HTTP requests. After completing this we should be able to type in the Raspberry Pi's public IP address (test on a different network i.e. using 4G or someone else's network) and it will redirect us to our website we just created via WordPress (note: this is now available for public consumption, next we want to map that public IP address to our domain name we will purchase). 

![Port-Forwarding](https://user-images.githubusercontent.com/36043248/87045938-62ac6b00-c1f0-11ea-8148-92f37c479fbc.PNG)

15. Purchase a domain from a domain registrar like DynaDot, NameCheap etc. I chose DynaDot, available @ https://www.dynadot.com/. In my case I purchased "jordanodonnell.me"
16. After your purchase your domain and it has been activated, navigate to https://www.dynu.com/en-US/ControlPanel/DDNS (create an account first it is **FREE**). Simply enter in your domain name (my case: "jordanodonnell.me") and ensure that "IPv4 address" is the same as the public IP address we retrived earlier from our Raspberry Pi using “*curl ifconfig.me*”. This will map your public IP address to your recently purchased domain name.

![Dynu DNS - Mapping Domain to IP](https://user-images.githubusercontent.com/36043248/87045096-625fa000-c1ef-11ea-9411-cf99c8846409.PNG)

17. After you have mapped your IP address to this domain name, we will then retrieve our nameservers via https://www.cloudflare.com/. Firstly sign up for an account (it is **FREE**) and then navigate to the "DNS" section after your see your domain profile. Ensure the IP address under the "Content" section is the same as that of the Pi yet again. Lastly, copy down the name servers (keep them somewhere safe). These are located where I have said "Name Server 1" and "Name Server 2".

![Cloudflare - NameServers](https://user-images.githubusercontent.com/36043248/87045160-773c3380-c1ef-11ea-96c2-093ca65a9cda.PNG)

18. Penultimately, we need to add those nameservers we wrote down to our account on DynaDot or the registrat you purchased your domain off. After signing in, simply navigate to "Account Info" or your profile. On the left hand-side under "My Domains" we want to click "Manage Domains". Click on the domain you want to add your nameservers to (in my case: "jordanodonnell.me") and then click the "DNS Settings". We will then enter the name of our two nameservers we recorded earlier.

![Adding NameServers to DynaDot (2)](https://user-images.githubusercontent.com/36043248/87045208-858a4f80-c1ef-11ea-8a70-d6f89c928592.PNG)

19. Finally, enter your Pi's public IP address into the address bar of Google Chrome followed by "/wp-admin/". I.e. "XXX.XX.XX.XXX/wp-admin/. We should see our wordpress website. Navigate to "Settings" and "General" and enter in your domain name for the "WordPress Address (URL)" and "Site Address (URL)". Click Save. After waiting a few minutes for changes to active, eimply enter in your domain into the address bar again i.e. "jordanodonnell.me" and you should now see that your domain has changed.

![WordPress Domain Change (Last)](https://user-images.githubusercontent.com/36043248/87045253-96d35c00-c1ef-11ea-8d43-54f08f23afd3.PNG)

VOILA! You have your own Raspberry Pi Web Server perfectly capable of Hosting Your Own Website. This solution is **WAY CHEAPER** than purchasing Web Hosting from vendors like “BlueHost” and “HostGator”. You also have the comfort, leisure and privacy to keep your **PERSONAL DATA**. 

