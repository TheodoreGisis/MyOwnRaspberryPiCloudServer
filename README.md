# MyOwnRaspberryPiCloudServer
First we need to update and upgrade our Raspberry pi:

       sudo apt update
       sudo apt dist-upgrade -y

After that reboot your system:

       sudo reboot
       
 Next timezones to yours by going it to raspi-config file: 
 
       sudo raspi-config
       
 When you enter inside Pi configuration click  Localisation Options -> Timezone  and select your timezone
       
 Install Webserver database and some libraries
 
       sudo apt install apache2 mariadb-server libapache2-mod-php -y
       
       sudo apt install php-gd php-json php-mysql php-curl php-mbstring php-intl php-imagick php-xml php-zip -y
 
 Next install NextCloud, link https://download.nextcloud.com/server/releases/nextcloud-22.2.0.zip and in terminal write 
 
       cd /var/www/html
       
  Then copy the above link with wget
  
       sudo wget https://download.nextcloud.com/server/releases/nextcloud-22.2.0.zip
       
  Unzip the file 
  
       sudo unzip nextcloud-22.1.0.zip
       
  Now change ownership
  
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
   
   Now we need to change directory and with nano we enter the default.conf file
   
       cd /etc/apache2/sites-available
       sudo nano 000-default.conf
      
    Inside we must change the "DocumentRoot /var/www/html/"  in to "DocumentRoot /var/www/html/nextcloud" and then reboot our system
    
       sudo reboot
       
    By puting your locan ip rasbperry address in to your browser you will open nextcloud,there create your account
    Next we add the external storage plugin in next cloud
    
       cd /media
       sudo mkdir pstorage
    
    Here we need to find our uid....Type:
    
       cut -d: -f1,3 /etc/passwd
       
      
   
       

