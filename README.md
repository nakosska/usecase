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


