# Snape-IT-Installation-on-Ubuntu-server-
Open source Inventory Managment tool  

# Install Ubuntu  Server 22.04 
<pre>
apt update 
apt install apache2 
assinged static IP to server 
</pre>




# To install Snipe-IT on Ubuntu Server, follow these steps carefully.

✅ STEP 1 — Update System
<pre>
sudo apt update && sudo apt upgrade -y </pre>


✅ STEP 2 — Install Apache
<pre>
sudo apt install apache2 -y
Enable & start:
sudo systemctl enable apache2
sudo systemctl start apache2 
</pre>



✅ STEP 3 — Install PHP (Required Version)
Snipe-IT needs PHP 8.1+
<pre>
sudo apt install php php-cli php-common php-curl php-mbstring php-xml php-bcmath php-zip php-gd php-mysql libapache2-mod-php unzip curl -y
</pre>
Check version:
<pre>
php -v </pre>


✅ STEP 4 — Install MariaDB (Database)
<pre>
    sudo apt install mariadb-server -y
    sudo mysql_secure_installation
</pre>

Answer:
Set root password → Yes
Remove anonymous users → Yes
Disallow root remote login → No
Remove test DB → Yes

✅ STEP 5 — Create Database for Snipe-IT
Login to MySQL:
<p> sudo mysql -u root -p </p> 
Inside MySQL:
<pre>CREATE DATABASE snipeit;
CREATE USER 'snipeuser'@'localhost' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON snipeit.* TO 'snipeuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
</pre>



✅ STEP 6 — Install Composer
<pre>

sudo apt install composer -y  </pre>

Check:
<pre> composer --version  </pre>

✅ STEP 7 — Download Snipe-IT
Go to web directory:
<pre>
cd /var/www
sudo git clone https://github.com/snipe/snipe-it snipeit
cd snipeit
    </pre>


✅ STEP 8 — Install PHP Dependencies
<p> sudo composer install --no-dev --prefer-source </p>

✅ STEP 9 — Configure Environment File
Copy example file:
<p >sudo cp .env.example .env </p>
Edit it:
<p >sudo nano .env </p>

Update these values:
<pre>
APP_URL=http://your-server-ip
DB_DATABASE=snipeit
DB_USERNAME=snipeuser
DB_PASSWORD=StrongPassword123!
    </pre>

Save & exit.

✅ STEP 10 — Generate App Key

<p> sudo php artisan key:generate</p>

✅ STEP 11 — Set Permissions
<pre>
sudo chown -R www-data:www-data /var/www/snipeit
sudo chmod -R 755 /var/www/snipeit
sudo chmod -R 775 storage
sudo chmod -R 775 public/uploads
    </pre>

✅ STEP 12 — Configure Apache Virtual Host
Create config:
<p >sudo nano /etc/apache2/sites-available/snipeit.conf </p>

Paste:
<pre>
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
    </pre>
Save.

Enable site & rewrite:
<pre>
sudo a2enmod rewrite
sudo a2ensite snipeit.conf
sudo a2dissite 000-default.conf
sudo systemctl restart apache2
    </pre>

✅ STEP 13 — Open in Browser
Go to:
<p >http://your-server-ip </p>
Follow the web installer to complete setup.

# 🎯 Done!
