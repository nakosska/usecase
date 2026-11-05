cat << 'EOF' > docker-compose.yml
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
