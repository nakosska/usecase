version: '3.8'

services:
  app:
    build: .
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    networks:
      - laravel_networkcat << 'EOF' > docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    networks:
      - laravel_network

  web:
    image: nginx:1.25-alpine
    container_name: laravel_web
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - .:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
    networks:
      - laravel_network

  db:
    image: mariadb:10.11
    container_name: laravel_db
    restart: unless-stopped
    environment:
      MARIADB_DATABASE: laravel
      MARIADB_USER: laravel_user
      MARIADB_PASSWORD: secret
      MARIADB_ROOT_PASSWORD: root_secret
    networks:
      - laravel_network

networks:
  laravel_network:
    driver: bridge
EOF



FROM php:8.1-fpm

RUN apt-get update && apt-get install -y \
    libpng-dev libonig-dev libxml2-dev zip unzip \
    && docker-php-ext-install pdo_mysql mbstring gd

WORKDIR /var/www


version: '3.8'

services:
  app:
    build: .
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    networks:
      - laravel_network

  web:
    image: nginx:1.25-alpine
    container_name: laravel_web
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - .:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
    networks:
      - laravel_network

  db:
    image: mariadb:10.11
    container_name: laravel_db
    restart: unless-stopped
    environment:
      MARIADB_DATABASE: laravel
      MARIADB_USER: laravel_user
      MARIADB_PASSWORD: secret
      MARIADB_ROOT_PASSWORD: root_secret
    networks:
      - laravel_network

networks:
  laravel_network:
    driver: bridge



version: '3.8'

services:
  app:
    build: .
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    networks:
      - laravel_network

  web:
    image: nginx:1.25-alpine
    container_name: laravel_web
    restart: unless-stopped
    ports:
      - "8080:80"
    volumes:
      - .:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
    networks:
      - laravel_network

  db:
    image: mariadb:10.11
    container_name: laravel_db
    restart: unless-stopped
    environment:
      MARIADB_DATABASE: laravel
      MARIADB_USER: laravel_user
      MARIADB_PASSWORD: secret
      MARIADB_ROOT_PASSWORD: root_secret
    networks:
      - laravel_network

networks:
  laravel_network:
    driver: bridge

    server {
    listen 80;
    root /var/www/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass app:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}



APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel_user
DB_PASSWORD=secret


mkdir -p ~/.docker/cli-plugins

curl -SL https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose

chmod +x ~/.docker/cli-plugins/docker-compose




printf "version: '3.8'\n\nservices:\n  app:\n    build: .\n    container_name: laravel_app\n    working_dir: /var/www\n    volumes:\n      - .:/var/www\n    networks:\n      - laravel_network\n\n  web:\n    image: nginx:1.25-alpine\n    container_name: laravel_web\n    restart: unless-stopped\n    ports:\n      - \"8080:80\"\n    volumes:\n      - .:/var/www\n      - ./nginx.conf:/etc/nginx/conf.d/default.conf\n    depends_on:\n      - app\n    networks:\n      - laravel_network\n\n  db:\n    image: mariadb:10.11\n    container_name: laravel_db\n    restart: unless-stopped\n    environment:\n      MARIADB_DATABASE: laravel\n      MARIADB_USER: laravel_user\n      MARIADB_PASSWORD: secret\n      MARIADB_ROOT_PASSWORD: root_secret\n    networks:\n      - laravel_network\n\nnetworks:\n  laravel_network:\n    driver: bridge\n" > docker-compose.yml




echo "dmVyc2lvbjogJzMuOCcKCnNlcnZpY2VzOgogIGFwcDoKICAgIGJ1aWxkOiAuCiAgICBjb250YWluZXJfbmFtZTogbGFyYXZlbF9hcHAKICAgIHdvcmtpbmdfZGlyOiAvdmFyL3d3d2ogICAgdm9sdW1lczoKICAgICAgLSAuOi92YXIvd3d3CiAgICBuZXR3b3JrczoKICAgICAgLSBsYXJhdmVsX25ldHdvcmsKCiAgd2ViOgogICAgaW1hZ2U6IG5naW54OjEuMjUtYWxwaW5lCiAgICBjb250YWluZXJfbmFtZTogbGFyYXZlbF93ZWIKICAgIHJlc3RhcnQ6IHVubGVzcy1zdG9wcGVkCiAgICBwb3J0czoKICAgICAgLSAiODA4MDo4MCIKICAgIHZvbHVtZXM6CiAgICAgIC0gLjovdmFyL3d3d2ogICAgICAtIC4vbmdpbnguY29uZjovZXRjL25naW54L2NvbmYuZC9kZWZhdWx0LmNvbmYKICAgIGRlcGVuZHNfb246CiAgICAgIC0gYXBwCiAgICBuZXR3b3JrczoKICAgICAgLSBsYXJhdmVsX25ldHdvcmsKCiAgZGI6CiAgICBpbWFnZTogbWFyaWFkYjoxMC4xMQogICAgY29udGFpbmVyX25hbWU6IGxhcmF2ZWxfZGIKICAgIHJlc3RhcnQ6IHVubGVzcy1zdG9wcGVkCiAgICBlbnZpcm9ubWVudDoKICAgICAgTUFSSUFEQl9EQVRBQkFTRTogbGFyYXZlbAogICAgICBNQVJJQURCX1VTRVI6IGxhcmF2ZWxfdXNlcgogICAgICBNQVJJQURCX1BBU1NXT1JEOiBzZWNyZXQKICAgICAgTUFSSUFEQl9ST09UX1BBU1NXT1JEOiByb290X3NlY3JldAogICAgbmV0d29ya3M6CiAgICAgIC0gbGFyYXZlbF9uZXR3b3JrCgpuZXR3b3JrczoKICBsYXJhdmVsX25ldHdvcms6CiAgICBkcml2ZXI6IGJyaWRnZQo=" | base64 -d > docker-compose.yml











echo "dmVyc2lvbjogJzMuOCcKCnNlcnZpY2VzOgogIGFwcDoKICAgIGJ1aWxkOiAuCiAgICBjb250YWluZXJfbmFtZTogbGFyYXZlbF9hcHAKICAgIHdvcmtpbmdfZGlyOiAvdmFyL3d3d2ogICAgdm9sdW1lczoKICAgICAgLSAuOi92YXIvd3d3CiAgICBuZXR3b3JrczoKICAgICAgLSBsYXJhdmVsX25ldHdvcmsKCiAgd2ViOgogICAgaW1hZ2U6IG5naW54OjEuMjUtYWxwaW5lCiAgICBjb250YWluZXJfbmFtZTogbGFyYXZlbF93ZWIKICAgIHJlc3RhcnQ6IHVubGVzcy1zdG9wcGVkCiAgICBwb3J0czoKICAgICAgLSAiODA4MDo4MCIKICAgIHZvbHVtZXM6CiAgICAgIC0gLjovdmFyL3d3d2ogICAgICAtIC4vbmdpbnguY29uZjovZXRjL25naW54L2NvbmYuZC9kZWZhdWx0LmNvbmYKICAgIGRlcGVuZHNfb246CiAgICAgIC0gYXBwCiAgICBuZXR3b3JrczoKICAgICAgLSBsYXJhdmVsX25ldHdvcmsKCiAgZGI6CiAgICBpbWFnZTogbWFyaWFkYjoxMC4xMQogICAgY29udGFpbmVyX25hbWU6IGxhcmF2ZWxfZGIKICAgIHJlc3RhcnQ6IHVubGVzcy1zdG9wcGVkCiAgICBlbnZpcm9ubWVudDoKICAgICAgTUFSSUFEQl9EQVRBQkFTRTogbGFyYXZlbAogICAgICBNQVJJQURCX1VTRVI6IGxhcmF2ZWxfdXNlcgogICAgICBNQVJJQURCX1BBU1NXT1JEOiBzZWNyZXQKICAgICAgTUFSSUFEQl9ST09UX1BBU1NXT1JEOiByb290X3NlY3JldAogICAgbmV0d29ya3M6CiAgICAgIC0gbGFyYXZlbF9uZXR3b3JrCgpuZXR3b3JrczoKICBsYXJhdmVsX25ldHdvcms6CiAgICBkcml2ZXI6IGJyaWRnZQo=" | base64 -d > docker-compose.yml



printf "version: '3.8'\n\nservices:\n  app:\n    build: .\n    container_name: laravel_app\n    working_dir: /var/www\n    volumes:\n      - .:/var/www\n    networks:\n      - laravel_network\n\n  web:\n    image: nginx:1.25-alpine\n    container_name: laravel_web\n    restart: unless-stopped\n    ports:\n      - \"8080:80\"\n    volumes:\n      - .:/var/www\n      - ./nginx.conf:/etc/nginx/conf.d/default.conf\n    depends_on:\n      - app\n    networks:\n      - laravel_network\n\n  db:\n    image: mariadb:10.11\n    container_name: laravel_db\n    restart: unless-stopped\n    environment:\n      MARIADB_DATABASE: laravel\n      MARIADB_USER: laravel_user\n      MARIADB_PASSWORD: secret\n      MARIADB_ROOT_PASSWORD: root_secret\n    networks:\n      - laravel_network\n\nnetworks:\n  laravel_network:\n    driver: bridge\n" > docker-compose.yml





echo "version: '3.8'" > docker-compose.yml
echo "services:" >> docker-compose.yml


echo "  app:" >> docker-compose.yml
echo "    build: ." >> docker-compose.yml
echo "    container_name: laravel_app" >> docker-compose.yml
echo "    working_dir: /var/www" >> docker-compose.yml
echo "    volumes:" >> docker-compose.yml
echo "      - .:/var/www" >> docker-compose.yml
echo "    networks:" >> docker-compose.yml
echo "      - laravel_network" >> docker-compose.yml


echo "  web:" >> docker-compose.yml
echo "    image: nginx:1.25-alpine" >> docker-compose.yml
echo "    container_name: laravel_web" >> docker-compose.yml
echo "    restart: unless-stopped" >> docker-compose.yml
echo "    ports:" >> docker-compose.yml
echo "      - \"8080:80\"" >> docker-compose.yml
echo "    volumes:" >> docker-compose.yml
echo "      - .:/var/www" >> docker-compose.yml
echo "      - ./nginx.conf:/etc/nginx/conf.d/default.conf" >> docker-compose.yml
echo "    depends_on:" >> docker-compose.yml
echo "      - app" >> docker-compose.yml
echo "    networks:" >> docker-compose.yml
echo "      - laravel_network" >> docker-compose.yml


echo "  db:" >> docker-compose.yml
echo "    image: mariadb:10.11" >> docker-compose.yml
echo "    container_name: laravel_db" >> docker-compose.yml
echo "    restart: unless-stopped" >> docker-compose.yml
echo "    environment:" >> docker-compose.yml
echo "      MARIADB_DATABASE: laravel" >> docker-compose.yml
echo "      MARIADB_USER: laravel_user" >> docker-compose.yml
echo "      MARIADB_PASSWORD: secret" >> docker-compose.yml
echo "      MARIADB_ROOT_PASSWORD: root_secret" >> docker-compose.yml
echo "    networks:" >> docker-compose.yml
echo "      - laravel_network" >> docker-compose.yml


echo "networks:" >> docker-compose.yml
echo "  laravel_network:" >> docker-compose.yml
echo "    driver: bridge" >> docker-compose.yml




version: '3.8'

services:
  app:
    build: .
    container_name: laravel_app
    working_dir: /var/www
    volumes:
      - .:/var/www
    networks:
      - laravel_network





python3 -c "print('''version: '3.8'\n\nservices:\n  app:\n    build: .\n    container_name: laravel_app\n    working_dir: /var/www\n    volumes:\n      - .:/var/www\n    networks:\n      - laravel_network\n\n  web:\n    image: nginx:1.25-alpine\n    container_name: laravel_web\n    restart: unless-stopped\n    ports:\n      - \"8080:80\"\n    volumes:\n      - .:/var/www\n      - ./nginx.conf:/etc/nginx/conf.d/default.conf\n    depends_on:\n      - app\n    networks:\n      - laravel_network\n\n  db:\n    image: mariadb:10.11\n    container_name: laravel_db\n    restart: unless-stopped\n    environment:\n      MARIADB_DATABASE: laravel\n      MARIADB_USER: laravel_user\n      MARIADB_PASSWORD: secret\n      MARIADB_ROOT_PASSWORD: root_secret\n    networks:\n      - laravel_network\n\nnetworks:\n  laravel_network:\n    driver: bridge''')" > docker-compose.yml





printf "server {\n    listen 80;\n    root /var/www/public;\n    index index.php index.html;\n\n    location / {\n        try_files \$uri \$uri/ /index.php?\$query_string;\n    }\n\n    location ~ \\.php\$ {\n        include fastcgi_params;\n        fastcgi_pass app:9000;\n        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;\n    }\n}\n" > nginx.conf


echo '<?php
define("LARAVEL_START", microtime(true));
require __DIR__."/vendor/autoload.php";
$app = require_once __DIR__."/bootstrap/app.php";
$kernel = $app->make(Illuminate\Contracts\Console\Kernel::class);
$status = $kernel->handle(
    $input = new Symfony\Component\Console\Input\ArgvInput,
    new Symfony\Component\Console\Output\ConsoleOutput
);
$kernel->terminate($input, $status);
exit($status);' > artisan






sudo apt install -y nginx mariadb-server php-fpm php-mysql php-curl php-gd php-mbstring php-xml php-zip php-intl php-bcmath php-imagick php-apcu



sudo nano /etc/nginx/sites-available/default

index index.php index.html index.htm index.nginx-debian.html;

location ^~ /phpmyadmin/ {
    alias /usr/share/phpmyadmin/;
    index index.php;
    location ~ ^/phpmyadmin/(.+\.php)$ {
        alias /usr/share/phpmyadmin/$1;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $request_filename;
    }
}


sudo ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/


sudo rm /etc/nginx/sites-enabled/default



CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'moodle_user'@'localhost' IDENTIFIED BY 'ПарольMoodle123';
GRANT ALL PRIVILEGES ON moodle.* TO 'moodle_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;


cd /tmp
sudo wget https://download.moodle.org/download.php/direct/stable500/moodle-latest-500.tgz
sudo tar -xzf moodle-latest-500.tgz
sudo mv moodle /var/www/html/moodle



sudo mkdir /var/moodledata
sudo chown -R www-data:www-data /var/moodledata
sudo chmod -R 0777 /var/moodledata
sudo chown -R www-data:www-data /var/www/html/moodle


sudo rm /etc/nginx/sites-enabled/phpmyadmin


sudo nano /etc/nginx/sites-available/moodle

server {
    listen 80;
    server_name _;
    root /var/www/html/moodle/public;
    index index.php index.html index.htm;

    # Настройки для Moodle (главная страница)
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ [^/]\.php(/|$) {
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }

    # Настройки для phpMyAdmin (по адресу 127.0.0.1:8080/phpmyadmin)
    location ^~ /phpmyadmin/ {
        alias /usr/share/phpmyadmin/;
        index index.php;
        location ~ ^/phpmyadmin/(.+\.php)$ {
            alias /usr/share/phpmyadmin/$1;
            fastcgi_pass unix:/run/php/php8.4-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }
}


sudo nginx -t
sudo systemctl reload nginx


SHOW GRANTS FOR 'moodleuser'@'localhost';

GRANT ALL PRIVILEGES ON moodle.* TO 'moodleuser'@'localhost';
FLUSH PRIVILEGES;

sudo -u www-data php -r "new mysqli('localhost', 'moodleuser', 'ваш_пароль', 'moodle'); echo 'OK';"

sudo -u www-data php /var/www/html/moodle/admin/cli/install.php --wwwroot="http://127.0.0.1:8080" --dataroot="/var/www/moodledata" --dbtype="mariadb" --dbhost="localhost" --dbname="moodle" --dbuser="moodleuser" --dbpass="ваш_пароль" --fullname="Moodle" --shortname="moodle" --adminpass="AdminPass123!" --non-interactive


sudo -u www-data php /var/www/html/moodle/admin/cli/install_database.php \
--adminpass="AdminPass123!" \
--adminemail="admin@example.com" \
--fullname="Moodle" \
--shortname="moodle" \
--agree-license


sudo -u www-data php /var/www/html/moodle/admin/cli/install_database.php --adminpass="AdminPass123!" --adminemail="admin@example.com" --fullname="Moodle" --shortname="moodle" --agree-license



server {
    listen 80;
    server_name _;
    root /var/www/html/moodle/public;
    index index.php index.html index.htm;

    location / {
        # Если файл или папка не найдены, перенаправляем на маршрутизатор Moodle
        try_files $uri $uri/ /r.php$is_args$args;
    }

    location ~ [^/]\.php(/|$) {
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }

    # Блок для phpMyAdmin (оставляем без изменений)
    location ^~ /phpmyadmin/ {
        alias /usr/share/phpmyadmin/;
        index index.php;
        location ~ ^/phpmyadmin/(.+\.php)$ {
            alias /usr/share/phpmyadmin/$1;
            fastcgi_pass unix:/run/php/php8.4-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }
}




sudo mkdir -p /etc/nginx/ssl
sudo cp private.key /etc/nginx/ssl/
sudo cp bundle.pem /etc/nginx/ssl/




# Этот блок перенаправляет всех, кто заходит по HTTP, на HTTPS
server {
    listen 80;
    server_name quinque.local;
    return 301 https://$host$request_uri;
}

# Основной блок с SSL
server {
    listen 443 ssl;
    server_name quinque.local;

    # Пути к нашим сертификатам
    ssl_certificate /etc/nginx/ssl/bundle.pem;
    ssl_certificate_key /etc/nginx/ssl/private.key;

    root /var/www/html/moodle;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ [^/]\.php(/|$) {
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        
        # Важно: сообщаем PHP, что соединение защищено
        fastcgi_param HTTPS on; 
    }
}




location ^~ /phpmyadmin/ {
    alias /usr/share/phpmyadmin/;
    index index.php;
    location ~ ^/phpmyadmin/(.+\.php)$ {
        alias /usr/share/phpmyadmin/$1;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $request_filename;
        fastcgi_param HTTPS on;
        fastcgi_param HTTP_X_FORWARDED_PROTO https;
    }
}



sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -m state --state RELATED,ESTABLISHED -j ACCEPT



Types: deb
URIs: http://deb.debian.org/debian
Suites: trixie trixie-updates
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg

Types: deb
URIs: http://security.debian.org/debian-security
Suites: trixie-security
Components: main contrib non-free non-free-firmware
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
