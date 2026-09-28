INSERT INTO glpi_users (name, password, is_active) VALUES ('admin', '$2y$10$p..X4No3kbl.9zq3s9yyXuuNdbHN78Bd/j8aiInj5L7Fo1Hg3hJMFa', 1);

INSERT INTO glpi_profiles_users (users_id, profiles_id, entities_id) SELECT id, 4, 0 FROM glpi_users WHERE name = 'admin';

sudo rm -rf /var/www/html/glpi/files/_cache/*
sudo systemctl restart apache2



sudo chown -R www-data:www-data /var/www/html/glpi

sudo find /var/www/html/glpi -type d -exec chmod 755 {} \;
sudo find /var/www/html/glpi -type f -exec chmod 644 {} \;


sudo rm -rf /var/www/html/glpi/files/_cache/*
sudo rm -rf /var/www/html/glpi/files/_sessions/*


sudo systemctl restart apache2
