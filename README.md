# MyOwnRaspberryPiCloudServer


<p align="center">
  <img width="460" height="350" src="https://github.com/TheodoreGisis/MyOwnRaspberryPiCloudServer/blob/main/Raspberry_cloud/My_raspberry_cloud.jpg" >
</p>



In this repository i will describe you, how i design my own cloud using a Raspberry pi zero.


First we need to update and upgrade our Raspberry pi:

       sudo apt update
       sudo apt dist-upgrade -y

After that reboot your system:

       sudo reboot
       
 Next timezones to yours by going to raspi-config file: 
 
       sudo raspi-config
       
 When you enter inside Pi configuration click  Localisation Options -> Timezone  and select your timezone
       
 Install Webserver database and some libraries:
 
       sudo apt install apache2 mariadb-server libapache2-mod-php -y
       
       sudo apt install php-gd php-json php-mysql php-curl php-mbstring php-intl php-imagick php-xml php-zip -y
 
 Next install NextCloud, link https://download.nextcloud.com/server/releases/nextcloud-22.2.0.zip and in terminal write: 
 
       cd /var/www/html
       
  Then copy the above link with wget:
  
       sudo wget https://download.nextcloud.com/server/releases/nextcloud-22.2.0.zip
       
  Unzip the file:
  
       sudo unzip nextcloud-22.1.0.zip
       
  Now change ownership:
  
       sudo chmod 750 nextcloud -R
       sudo chown www-data:www-data nextcloud -R
       
   The next step is to create the database for nextcloud(type one by one the following commands):
       
       sudo mysql
       CREATE USER 'nextcloud' IDENTIFIED BY 'password';
       CREATE DATABASE nextcloud;
       GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@localhost IDENTIFIED BY 'password';
       FLUSH PRIVILEGES;
       quit
       sudo reboot
   
   Now we need to change directory and with nano we enter the default.conf file:
   
       cd /etc/apache2/sites-available
       sudo nano 000-default.conf
      
   Inside we must change the "DocumentRoot /var/www/html/"  in to "DocumentRoot /var/www/html/nextcloud" and then reboot our system
    
       sudo reboot
       
   By puting your local ip raspberry address in to your browser you will open nextcloud.Tthere create your account.
   Next we add the external storage(8 GB USB) plugin in next cloud:
    
       cd /media
       sudo mkdir pstorage
    
   Here we need to find our uid....Type:
    
       cut -d: -f1,3 /etc/passwd
   
   Now look and find the one who stats with "www ..." and cut the id
   
   After that find the gid's by typing:
   
       getent group
      
   Look and find again the "www ..." and cut the id:
   
   Next type:
   
       sudo blkid
    
   Here cut the 'UUID'.
   
   Finally go with nano  to: 
   
       sudo nano /etc/fstab
       
   At the end paste:  UUID=------- /media/pstorage auto defaults,uid=-------gid=------ 0 2  and then:
   
       sudo mount -a
       sudo reboot
       df -h
   Open nextcloud in browser:
   
   Now the last step is to enable external storage.
   
   You will go to your Nextclound-> Setings ->Search for External Storage -> Enable.
   
   Next go to Settings ->External Storage -> Give a folder name -> In external storage select "local" ->in configuration type "/media/pistorage" and you are ready.
   
   Now lets secure SSL!
   
     sudo mkdir -p /etc/apache2/ssl
     
   Generate the certificate:
      
      sudo openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
      
   Enable SSL Module:
      
      sudo a2enmod ssl
      sudo systemctl restart apache2
      
   Modify the configuration file:
    
       cd /etc/apache2/sites-available
       sudo nano default-ssl.conf
       
   Make the change "DocumentRoot /var/www/html/nextcloud" ,"SSLCertificateFile /etc/apache2/ssl/apache.crt", "SSLCertificateKeyFile /etc/apache2/ssl/apache.key"
    
   Enable default SSL and restart apache
      
        sudo a2ensite default-ssl.conf
        sudo service apache2 restar

   Force SSL usage on NextCloud
    
        sudo nano /etc/apache2/sites-available/000-default.conf
        
   Above "ErrorLog ${APACHE_LOG_DIR}/error.log" put this: 
    
       RewriteEngine On
     
       RewriteCond %{HTTPS} off
    
       RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
        
   And then 
    
      sudo a2enmod rewrite
      sudo service apache2 restart
      sudo reboot
      
   And its done.You have official create your own cloud server!
