DROP DATABASE IF EXISTS glpi;
CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
ALTER USER 'glpi'@'localhost' IDENTIFIED BY 'debian';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
EXIT;


sudo chown -R www-data:www-data /var/www/html/glpi
sudo find /var/www/html/glpi -type d -exec chmod 755 {} +
sudo find /var/www/html/glpi -type f -exec chmod 644 {} +
sudo chmod -R 775 /var/www/html/glpi/files
sudo chmod 755 /var /var/www /var/www/html


sudo ln -s /var/www/html/glpi/install /var/www/html/glpi/public/install
sudo systemctl restart apache2

