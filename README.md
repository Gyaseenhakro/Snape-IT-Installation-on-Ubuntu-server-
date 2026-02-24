# Snape-IT-Installation-on-Ubuntu-server-
Open source Inventory Managment tool  

# Install Ubuntu  Server 22.04 
< a >
apt update 
apt install apache2 
assinged static IP to server 
</a >



# To install Snipe-IT on Ubuntu Server, follow these steps carefully.

✅ STEP 1 — Update System
sudo apt update && sudo apt upgrade -y


✅ STEP 2 — Install Apache
<p>
sudo apt install apache2 -y
Enable & start:
sudo systemctl enable apache2
sudo systemctl start apache2 
</p>



✅ STEP 3 — Install PHP (Required Version)
Snipe-IT needs PHP 8.1+
sudo apt install php php-cli php-common php-curl php-mbstring php-xml php-bcmath php-zip php-gd php-mysql libapache2-mod-php unzip curl -y
Check version:
php -v 


✅ STEP 4 — Install MariaDB (Database)
sudo apt install mariadb-server -y
Secure it:
sudo mysql_secure_installation
Answer:
Set root password → Yes
Remove anonymous users → Yes
Disallow root remote login → No
Remove test DB → Yes

✅ STEP 5 — Create Database for Snipe-IT
Login to MySQL:
sudo mysql -u root -p
Inside MySQL:
CREATE DATABASE snipeit;
CREATE USER 'snipeuser'@'localhost' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON snipeit.* TO 'snipeuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;


✅ STEP 6 — Install Composer
sudo apt install composer -y
Check:
composer --version

✅ STEP 7 — Download Snipe-IT
Go to web directory:
cd /var/www
Download:
sudo git clone https://github.com/snipe/snipe-it snipeit
Go inside:
cd snipeit


✅ STEP 8 — Install PHP Dependencies
sudo composer install --no-dev --prefer-source

✅ STEP 9 — Configure Environment File
Copy example file:
sudo cp .env.example .env
Edit it:
sudo nano .env

Update these values:

APP_URL=http://your-server-ip
DB_DATABASE=snipeit
DB_USERNAME=snipeuser
DB_PASSWORD=StrongPassword123!

Save & exit.

✅ STEP 10 — Generate App Key
sudo php artisan key:generate

✅ STEP 11 — Set Permissions
sudo chown -R www-data:www-data /var/www/snipeit
sudo chmod -R 755 /var/www/snipeit
sudo chmod -R 775 storage
sudo chmod -R 775 public/uploads

✅ STEP 12 — Configure Apache Virtual Host
Create config:
sudo nano /etc/apache2/sites-available/snipeit.conf

Paste:
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/snipeit/public
    ServerName your-server-ip

    <Directory /var/www/snipeit/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/snipeit_error.log
    CustomLog ${APACHE_LOG_DIR}/snipeit_access.log combined
</VirtualHost>
Save.

Enable site & rewrite:

sudo a2enmod rewrite
sudo a2ensite snipeit.conf
sudo a2dissite 000-default.conf
sudo systemctl restart apache2

✅ STEP 13 — Open in Browser
Go to:
http://your-server-ip
Follow the web installer to complete setup.

# 🎯 Done!
