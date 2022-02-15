# CraftCMS in Docker & Docker-Slim [![Github Packages CI](https://github.com/antyung88/docker-craftcms/actions/workflows/master.yml/badge.svg)](https://github.com/antyung88/docker-craftcms/actions/workflows/master.yml)
Run Craft CMS in Docker & Docker-Slim 

```
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
ghcr.io/antyung88/craftcms.slim   latest    5b23ad70a9ce   1 minutes ago   157MB
ghcr.io/antyung88/craftcms        master    33c597a8d003   1 minutes ago   974MB
```

## Prerequisites
- Ubuntu (Recommended)
- Docker
- Docker-Compose
- Traefik

---

## Docker Compose (Fat)

```
version: '3.3'
services:
  craftcms:
    image: ghcr.io/antyung88/craftcms:master
    env_file:
      - .env
    restart: always
    volumes:
      - ./html/assets:/var/www/html/web/assets:z
      - ./html/backups:/var/www/html/storage/backups
      - ./html/templates:/var/www/html/templates
      - ./html/translations:/var/www/html/translations
      - ./html/redactor:/var/www/html/config/redactor
    networks:
      - traefik-public
    labels:
      - traefik.enable=true
      - traefik.docker.network=traefik-public
      - traefik.constraint-label=traefik-public
      - traefik.http.routers.craftcms-http.rule=Host(`craft.example.com`)
      - traefik.http.routers.craftcms-http.entrypoints=http
      - traefik.http.routers.craftcms-http.middlewares=https-redirect
      - traefik.http.routers.craftcms-https.rule=Host(`craft.example.com`)
      - traefik.http.routers.craftcms-https.entrypoints=https
      - traefik.http.routers.craftcms-https.tls=true
      - traefik.http.routers.craftcms-https.tls.certresolver=le
      - traefik.http.services.craftcms.loadbalancer.server.port=8080
    depends_on:
      - db

  db:
    image: mariadb:10
    restart: always
    volumes:
      - ./db-data:/var/lib/mysql
    networks:
      - traefik-public
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_ROOT_PASSWORD: examplepass

networks:
  traefik-public:
    external: true
```

.env
```
# The environment Craft is currently running in (dev, staging, production, etc.)
ENVIRONMENT=dev

# The application ID used to to uniquely store session and cache data, mutex locks, and more
APP_ID=

# The secure key Craft will use for hashing and encrypting data
SECURITY_KEY=

# Database Configuration
DB_DRIVER=mysql
DB_SERVER=db
DB_PORT=3306
DB_DATABASE=exampledb
DB_USER=exampleuser
DB_PASSWORD=examplepass
DB_SCHEMA=public
DB_TABLE_PREFIX=

# The URI segment that tells Craft to load the control panel
CP_TRIGGER=admin
```


```
docker-compose up -d
```

---

# Docker Compose (Slim) 

## TESTING PHASE DO NOT USE IN PRODUCTION

```
version: '3.3'
services:
  craftcms:
    image: ghcr.io/antyung88/craftcms.slim:latest
    restart: always
    env_file:
      - .env
    restart: always
    volumes:
      - ./html/assets:/var/www/html/web/assets:z
      - ./html/backups:/var/www/html/storage/backups
      - ./html/templates:/var/www/html/templates
      - ./html/translations:/var/www/html/translations
      - ./html/redactor:/var/www/html/config/redactor
    networks:
      - traefik-public
    labels:
      - traefik.enable=true
      - traefik.docker.network=traefik-public
      - traefik.constraint-label=traefik-public
      - traefik.http.routers.craftcms-http.rule=Host(`craft.example.com`)
      - traefik.http.routers.craftcms-http.entrypoints=http
      - traefik.http.routers.craftcms-http.middlewares=https-redirect
      - traefik.http.routers.craftcms-https.rule=Host(`craft.example.com`)
      - traefik.http.routers.craftcms-https.entrypoints=https
      - traefik.http.routers.craftcms-https.tls=true
      - traefik.http.routers.craftcms-https.tls.certresolver=le
      - traefik.http.services.craftcms.loadbalancer.server.port=8080
    depends_on:
      - db

  db:
    image: mariadb:10
    restart: always
    volumes:
      - ./db-data:/var/lib/mysql
    networks:
      - traefik-public
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_ROOT_PASSWORD: examplepass

networks:
  traefik-public:
    external: true
```

.env
```
# The environment Craft is currently running in (dev, staging, production, etc.)
ENVIRONMENT=dev

# The application ID used to to uniquely store session and cache data, mutex locks, and more
APP_ID=

# The secure key Craft will use for hashing and encrypting data
SECURITY_KEY=

# Database Configuration
DB_DRIVER=mysql
DB_SERVER=db
DB_PORT=3306
DB_DATABASE=exampledb
DB_USER=exampleuser
DB_PASSWORD=examplepass
DB_SCHEMA=public
DB_TABLE_PREFIX=

# The URI segment that tells Craft to load the control panel
CP_TRIGGER=admin
```

```
docker-compose up -d
```
