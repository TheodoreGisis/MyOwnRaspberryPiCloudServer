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
       

