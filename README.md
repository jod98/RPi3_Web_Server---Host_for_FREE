# RPi3_Web_Server (Host Your Website for FREE)

#### With a keen interest in web development, I wanted to better understand how one could “make a website”. After investigating into the prices of hosting a domain, I decided to create my own web server using a Raspberry Pi 3 I had (living the stereotypical student life I know). This project exposed me to the likes of Apache, PHP, MySQL and HTML. This solution is cheaper than purchasing web hosting, I have the comfort of mind knowing my personal data is not being distributed globally and best of all, I learnt a lot. 

#### My Website: 

![Website](https://user-images.githubusercontent.com/36043248/104812522-b381c980-57fa-11eb-886d-c49ff65a9087.PNG)

-------------------------------------------------------------------------------------------------------------------------------

1. Download Raspian Lite @ https://www.raspberrypi.org/downloads/raspberry-pi-os/
2. Flash the “.zip” file onto your SD card (preferably 8GB or greater) using Etcher @ https://www.balena.io/etcher/
3. Insert SD card into Raspberry Pi and connect it to your router via Ethernet cable.
4. Connect the Pi to your TV/monitor via a HDMI cable and login using 
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

14. We want to forward a port on our router using "Port Forwarding". We can access this section by logging into our router, usually available @ "192.168.1.1" and navigating to the "Port Forwarding" or "Port Mapping" section. We simply want to forward the port of Raspbery Pi's private IP address. Enter "80" for both the "External port" and "Internal port" as this is the port dedictated to HTTP requests. After completing this we should be able to type in the Raspberry Pi's public IP address into the Google Chrome address bar (test on a different network i.e. using 4G or someone else's network) and it will redirect us to our website we just created via WordPress. 

![Port-Forwarding](https://user-images.githubusercontent.com/36043248/87045938-62ac6b00-c1f0-11ea-8148-92f37c479fbc.PNG)

14.(a) Repeat this process but this time we want to enter port number "443". This is required for a future step, allowing us to redirect traffic from "http" to "https" requests. In simple terms, this will make the website more secure by encrypting data. N.B. Ensure you reset your router/modem after this process so the changes can take effect.

![Port Forwarding - 2](https://user-images.githubusercontent.com/36043248/104127868-716d0980-535c-11eb-8494-2bf5bf484b7f.PNG)

15. Purchase a domain from a domain registrar like DynaDot, NameCheap etc. I chose DynaDot, available @ https://www.dynadot.com/. In my case I purchased "jordanodonnell.me"

16. After you purchase your domain and it has been activated you will map your IP address to this domain name. we will then retrieve our nameservers via https://www.cloudflare.com/. Firstly sign up for an account (it is **FREE**) and then navigate to the "DNS" section after clicking your domain profile. Ensure the IP address under the "Content" section is the same as Pi's public IP yet again. Lastly, copy down the nameservers (keep them somewhere safe). These are located where I have said "Name Server 1" and "Name Server 2".

![Cloudflare - NameServers](https://user-images.githubusercontent.com/36043248/87045160-773c3380-c1ef-11ea-96c2-093ca65a9cda.PNG)

17. We need to add those nameservers we wrote down to our account on DynaDot or the registrar you purchased your domain off. After signing in, simply navigate to "Account Info" or your profile. On the left hand-side under "My Domains" we want to click "Manage Domains". Click on the domain you want to add your nameservers to (in my case: "jordanodonnell.me") and then click "DNS Settings". Click the drop down menu and change "DynaDot Parking" to "Name Servers". Next, we will then enter the name of our two nameservers we recorded earlier.

![Adding NameServers to DynaDot (2)](https://user-images.githubusercontent.com/36043248/87045208-858a4f80-c1ef-11ea-8a70-d6f89c928592.PNG)

18. Enter your Pi's public IP address into the address bar of Google Chrome followed by "/wp-admin/" i.e. "XXX.XX.XX.XXX/wp-admin/". We should see our wordpress website. Navigate to "Settings" and "General" and enter in your domain name for the "WordPress Address (URL)" and "Site Address (URL)". Click Save. After waiting a few minutes for changes to active, simply enter in your domain into the address bar again i.e. "jordanodonnell.me" and you should now see that your domain has changed.

![WordPress Domain Change (Last)](https://user-images.githubusercontent.com/36043248/87045253-96d35c00-c1ef-11ea-8d43-54f08f23afd3.PNG)

19. Next want to dyanmically update our public IP which is linked to our domain on CloudFlare. The reason we want to do this is because if your router resets as a result of a power outtage the public IP will change. This will cause your domain name to no longer be mapped to your public IP meaning you won't be able to access your website via your domain name i.e. "jordanodonnell.me". To solve this issue, we want to retrieve a free temporary hostname which will dyanmically retrieve the updated IP address and then re-map that to your domain. Simply sign up with an account with NO-IP and navigate to this page: https://my.noip.com/#!/dynamic-dns. On this page you will need to retrieve a free hostname and map that to your public IP as you can see in the screenshot below. 

![NO-IP](https://user-images.githubusercontent.com/36043248/87309126-15900800-c514-11ea-887f-4b12ff6c26c8.PNG)

20. We want to now reconfigure our CloudFlare settings. Navigate to the "DNS" section like before by this time we want to delete our two 'A' records and add just a "CNAME" record like in the screenshot below. You then want to add your domain name under the "Name" column and your new hostname you retreived in the last step under the "Content" column.

![CloudFlare](https://user-images.githubusercontent.com/36043248/87309439-8505f780-c514-11ea-881a-e61391a2f928.PNG)

21. (NO LONGER NECESSARY) 𝙵̶̶̶𝚒̶̶̶𝚗̶̶̶𝚊̶̶̶𝚕̶̶̶𝚕̶̶̶𝚢̶̶̶,̶̶̶ ̶̶̶𝙸̶̶̶𝚗̶̶̶ ̶̶̶𝚘̶̶̶𝚛̶̶̶𝚍̶̶̶𝚎̶̶̶𝚛̶̶̶ ̶̶̶𝚝̶̶̶𝚘̶̶̶ ̶̶̶𝚍̶̶̶𝚢̶̶̶𝚗̶̶̶𝚊̶̶̶𝚖̶̶̶𝚒̶̶̶𝚌̶̶̶𝚊̶̶̶𝚕̶̶̶𝚕̶̶̶𝚢̶̶̶ ̶̶̶𝚛̶̶̶𝚎̶̶̶-̶̶̶𝚖̶̶̶𝚊̶̶̶𝚙̶̶̶ ̶̶̶𝚘̶̶̶𝚞̶̶̶𝚛̶̶̶ ̶̶̶𝚍̶̶̶𝚘̶̶̶𝚖̶̶̶𝚊̶̶̶𝚒̶̶̶𝚗̶̶̶ ̶̶̶𝚝̶̶̶𝚘̶̶̶ ̶̶̶𝚝̶̶̶𝚑̶̶̶𝚎̶̶̶ ̶̶̶𝚗̶̶̶𝚎̶̶̶𝚠̶̶̶ ̶̶̶𝚙̶̶̶𝚞̶̶̶𝚋̶̶̶𝚕̶̶̶𝚒̶̶̶𝚌̶̶̶ ̶̶̶𝙸̶̶̶𝙿̶̶̶ ̶̶̶𝚒̶̶̶𝚗̶̶̶ ̶̶̶𝚊̶̶̶𝚗̶̶̶ ̶̶̶𝚎̶̶̶𝚟̶̶̶𝚎̶̶̶𝚗̶̶̶𝚝̶̶̶ ̶̶̶𝚘̶̶̶𝚏̶̶̶ ̶̶̶𝚘̶̶̶𝚞̶̶̶𝚛̶̶̶ ̶̶̶𝚛̶̶̶𝚘̶̶̶𝚞̶̶̶𝚝̶̶̶𝚎̶̶̶𝚛̶̶̶ ̶̶̶𝚛̶̶̶𝚎̶̶̶𝚜̶̶̶𝚎̶̶̶𝚝̶̶̶𝚝̶̶̶𝚒̶̶̶𝚗̶̶̶𝚐̶̶̶,̶̶̶ ̶̶̶𝚠̶̶̶𝚎̶̶̶ ̶̶̶𝚗̶̶̶𝚎̶̶̶𝚎̶̶̶𝚍̶̶̶ ̶̶̶𝚝̶̶̶𝚘̶̶̶ ̶̶̶𝚒̶̶̶𝚗̶̶̶𝚜̶̶̶𝚝̶̶̶𝚊̶̶̶𝚕̶̶̶𝚕̶̶̶ ̶̶̶𝙽̶̶̶𝙾̶̶̶-̶̶̶𝙸̶̶̶𝙿̶̶̶ ̶̶̶𝚌̶̶̶𝚕̶̶̶𝚒̶̶̶𝚎̶̶̶𝚗̶̶̶𝚝̶̶̶ ̶̶̶𝚘̶̶̶𝚗̶̶̶ ̶̶̶𝚘̶̶̶𝚞̶̶̶𝚛̶̶̶ ̶̶̶𝚁̶̶̶𝚊̶̶̶𝚜̶̶̶𝚙̶̶̶𝚋̶̶̶𝚎̶̶̶𝚛̶̶̶𝚛̶̶̶𝚢̶̶̶ ̶̶̶𝙿̶̶̶𝚒̶̶̶.̶̶̶ ̶̶̶𝙵̶̶̶𝚘̶̶̶𝚕̶̶̶𝚕̶̶̶𝚘̶̶̶𝚠̶̶̶ ̶̶̶𝚝̶̶̶𝚑̶̶̶𝚒̶̶̶𝚜̶̶̶ ̶̶̶𝚐̶̶̶𝚞̶̶̶𝚒̶̶̶𝚍̶̶̶𝚎̶̶̶ ̶̶̶𝚑̶̶̶𝚎̶̶̶𝚛̶̶̶𝚎̶̶̶:̶̶̶ ̶̶̶𝚑̶̶̶𝚝̶̶̶𝚝̶̶̶𝚙̶̶̶𝚜̶̶̶:̶̶̶/̶̶̶/̶̶̶𝚒̶̶̶𝚟̶̶̶𝚊̶̶̶𝚗̶̶̶𝚌̶̶̶𝚊̶̶̶𝚛̶̶̶𝚘̶̶̶𝚜̶̶̶𝚊̶̶̶𝚝̶̶̶𝚒̶̶̶.̶̶̶𝚌̶̶̶𝚘̶̶̶𝚖̶̶̶/̶̶̶𝚗̶̶̶𝚘̶̶̶-̶̶̶𝚒̶̶̶𝚙̶̶̶-̶̶̶𝚠̶̶̶𝚒̶̶̶𝚝̶̶̶𝚑̶̶̶-̶̶̶𝚛̶̶̶𝚊̶̶̶𝚜̶̶̶𝚙̶̶̶𝚋̶̶̶𝚎̶̶̶𝚛̶̶̶𝚛̶̶̶𝚢̶̶̶-̶̶̶𝚙̶̶̶𝚒̶̶̶/̶̶̶.̶̶̶ ̶̶̶𝙰̶̶̶𝚏̶̶̶𝚝̶̶̶𝚎̶̶̶𝚛̶̶̶ ̶̶̶𝚌̶̶̶𝚘̶̶̶𝚖̶̶̶𝚙̶̶̶𝚕̶̶̶𝚎̶̶̶𝚝̶̶̶𝚒̶̶̶𝚗̶̶̶𝚐̶̶̶ ̶̶̶𝚝̶̶̶𝚑̶̶̶𝚒̶̶̶𝚜̶̶̶ ̶̶̶𝚐̶̶̶𝚞̶̶̶𝚒̶̶̶𝚍̶̶̶𝚎̶̶̶,̶̶̶ ̶̶̶𝚛̶̶̶𝚎̶̶̶𝚜̶̶̶𝚎̶̶̶𝚝̶̶̶ ̶̶̶𝚢̶̶̶𝚘̶̶̶𝚞̶̶̶𝚛̶̶̶ ̶̶̶𝚛̶̶̶𝚘̶̶̶𝚞̶̶̶𝚝̶̶̶𝚎̶̶̶𝚛̶̶̶ ̶̶̶𝚊̶̶̶𝚗̶̶̶𝚍̶̶̶ ̶̶̶𝚒̶̶̶𝚗̶̶̶𝚙̶̶̶𝚞̶̶̶𝚝̶̶̶ ̶̶̶𝚢̶̶̶𝚘̶̶̶𝚞̶̶̶𝚛̶̶̶ ̶̶̶𝚍̶̶̶𝚘̶̶̶𝚖̶̶̶𝚊̶̶̶𝚒̶̶̶𝚗̶̶̶ ̶̶̶𝚗̶̶̶𝚊̶̶̶𝚖̶̶̶𝚎̶̶̶ ̶̶̶𝚒̶̶̶𝚗̶̶̶ ̶̶̶𝚝̶̶̶𝚘̶̶̶ ̶̶̶𝚝̶̶̶𝚑̶̶̶𝚎̶̶̶ ̶̶̶𝙶̶̶̶𝚘̶̶̶𝚘̶̶̶𝚐̶̶̶𝚕̶̶̶𝚎̶̶̶ ̶̶̶𝙲̶̶̶𝚑̶̶̶𝚛̶̶̶𝚘̶̶̶𝚖̶̶̶𝚎̶̶̶ ̶̶̶𝚊̶̶̶𝚍̶̶̶𝚍̶̶̶𝚛̶̶̶𝚎̶̶̶𝚜̶̶̶𝚜̶̶̶ ̶̶̶𝚋̶̶̶𝚊̶̶̶𝚛̶̶̶ ̶̶̶𝚝̶̶̶𝚘̶̶̶ ̶̶̶𝚒̶̶̶𝚝̶̶̶ ̶̶̶𝚠̶̶̶𝚘̶̶̶𝚛̶̶̶𝚔̶̶̶𝚜̶̶̶.̶̶̶

21. Before you begin editing your website, I encourage you to set up SSL. This is the "lock" icon you see which informs the user that the website is "secure" which encrypts all your personal data you enter into the site like credit card details, phone numbers etc. As a result your website will have the "https://" initial address. To begin setting up SSL follow this guide: https://variax.wordpress.com/2017/03/18/adding-https-to-the-raspberry-pi-apache-web-server/comment-page-1/. NOTE: The 'FDQN' name is simply your domain name i.e. "jordanodonnell.me". If you experience any issues with certain commands, please refer to the comments section below in the guide. After you complete the guide you will want to restart your Apache web server by issuing the following command: “*sudo systemctl restart apache2*”. Lastly, follow this portion of the YouTube video to finalise setting up SSL: https://youtu.be/8AZ8GqW5iak?t=4073

![SSL](https://user-images.githubusercontent.com/36043248/87311151-c7c8cf00-c516-11ea-93fd-971c7f727360.PNG)

22. Lastly, navigate to the general settings tab of your website's admin page. Enter in your domain name ensuring you include 'https://' at the beginning as we have successfully encrypted the data transmitted over this domain in the previous step. 

![General Settings - Wordpress](https://user-images.githubusercontent.com/36043248/104128776-5d77d680-5361-11eb-9639-7a6dafeb788d.PNG)

23. To being editing your wordpress website, I highly recommend this YouTube video: https://youtu.be/8AZ8GqW5iak.

24. To avoid potential issues in the future through updated themes/plugins, it is recommended that you create a backup of your website. These guides demonstrate how you can begin to schedule routinely backups: https://www.youtube.com/watch?v=bmx39y_8tOs&ab_channel=CreateaProWebsite and https://medium.com/@christoph.schmidl/how-to-manually-backup-wordpress-daa43e37a9bd

VOILA! You have your own Raspberry Pi Web Server perfectly capable of Hosting Your Own Website. This solution is **WAY CHEAPER** than purchasing Web Hosting from vendors like “BlueHost” and “HostGator”. You also have the comfort, leisure and privacy to keep your **PERSONAL DATA**. 

PS. During the process you may receive errors relating to 'XMLReader Extension' and 'cURL support' when importing a website. Simply issue these commands into the terminal to bypass these errors; 'sudo apt install php7.3-xml' and 'sudo apt-get install php7.3-curl'

PSS. Don't change post permalinks to 'post name' at beginning. Leave as default




