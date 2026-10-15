sudo apt update
sudo apt install -y lsb-release ca-certificates curl gnupg
curl -sSLo /tmp/debsuryorg-archive-keyring.deb https://packages.sury.org/debsuryorg-archive-keyring.deb
sudo dpkg -i /tmp/debsuryorg-archive-keyring.deb
echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
sudo apt update
sudo apt install -y php8.3 libapache2-mod-php8.3 php8.3-mysql php8.3-curl php8.3-gd php8.3-imap php8.3-ldap php8.3-xml php8.3-mbstring php8.3-xmlrpc php8.3-zip php8.3-bcmath php8.3-intl php8.3-redis


sudo a2dismod php8.4
sudo a2enmod php8.3
sudo systemctl restart apache2
php -v


sudo /home/debian/backup_glpi.sh


cd /tmp
sudo wget -O glpi12.tgz https://github.com/glpi-project/glpi/releases/download/12.0.0/glpi-12.0.0.tgz
sudo tar -zxvf glpi12.tgz


sudo cp -r /var/www/html/glpi/config /tmp/glpi_config_backup
sudo cp -r /var/www/html/glpi/files /tmp/glpi_files_backup


sudo mv /var/www/html/glpi /var/www/html/glpi_old
sudo mv /tmp/glpi /var/www/html/glpi
sudo cp -r /tmp/glpi_config_backup/* /var/www/html/glpi/config/
sudo cp -r /tmp/glpi_files_backup/* /var/www/html/glpi/files/


sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
sudo chmod -R 775 /var/www/html/glpi/files
sudo systemctl restart apache2





ls -d /tmp/glpi_config_backup /tmp/glpi_files_backup /var/www/html/glpi_old /var/www/html/glpi


ls -lh /backup/glpi/


sudo tar -xzvf /backup/glpi/glpi_files_2026-10-14.tar.gz -C /tmp/


ls /var/www/html/glpi


sudo cp -r /tmp/glpi_config_backup/* /var/www/html/glpi/config/
sudo cp -r /tmp/glpi_files_backup/* 





/var/www/html/glpi/files/

sudo chown -R www-data:www-data /var/www/html/glpi
sudo chmod -R 755 /var/www/html/glpi
sudo chmod -R 775 /var/www/html/glpi/files
sudo systemctl restart apache2



sudo ln -s /var/www/html/glpi/install /var/www/html/glpi/public/install
sudo systemctl restart apache2






<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>




ls -la /var/www/html/glpi/public/install

sudo ln -s /var/www/html/glpi/install /var/www/html/glpi/public/install

cat /etc/apache2/sites-available/glpi.conf


sudo systemctl restart apache2



sudo nano /var/www/html/glpi/public/.htaccess


<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule ^install/(.*)$ index.php [QSA,L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>


sudo rm -f /var/www/html/glpi/public/install

sudo systemctl restart apache2
