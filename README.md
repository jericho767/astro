# Docker Setup for Laravel API and/or ReactJS
A boilerplate for dockerized implementation of both Laravel and ReactJS* applications.  
<sup>* Other frameworks may work but not yet tested.</sup>

## Specifications / Infrastructure Information
- Nginx
- PHP-FPM
- MySQL
- Postfix
- CS-Fixer
- Data Volume
- Composer
- Cron
- Node/NPM
- Redis

## Prerequisites
- git
- docker
- docker-compose

## Getting Started
Copy `.env` for docker in root directory
```
cp .env.example .env
```
Fill in variables
```
ENVIRONMENT=development                 # development/staging/production
PROJECT_NAME=YOUR_PROJECT_NAME_HERE     # Prefix for the docker containers to be created
MYSQL_ROOT_PASSWORD=p@ss1234!           # root password of the root mysql user
MYSQL_DATABASE=YOUR_DATABASE_NAME       # database that will be created
MYSQL_USER=YOUR_DATABASE_USER           # mysql user
MYSQL_PASSWORD=YOUR_DATABASE_USER       # user password
....
....
....
API_DOMAIN=api.app.local // for local development
APP_DOMAIN=app.local // for local development
```
For local development, add the following lines to host file
```
192.168.99.100    app.local api.app.local
```
Note: Replace `192.168.99.100` with your docker machine IP.

## Build the all containers
```
docker-compose build
```
Start the containers
```
docker-compose up -d
```
Run the following command to login to any docker container
```
docker exec -it CONTAINER_NAME bash
```
## Setting up Laravel
Clone/Checkout/Paste laravel code inside `src/backend`.
```
cd src && git clone https://github.com/laravel/laravel.git backend
```
Change `https://github.com/laravel/laravel.git` to your repository.

Install the composer packages
```
docker-compose run composer install
```
Create the `.env` file and run the following to generate key for Laravel
```
docker-compose run php cp .env.example .env
docker-compose run php php artisan key:generate
```
Update the `.env` values especially the database credentials then refresh the config
```
docker-compose run php php artisan config:clear
```
Run the migration
```
docker-compose run php php artisan migrate:fresh
```
If you have seeders, you can run it by using the following command
```
docker-compose run php php artisan db:seed
```

## PSR2 Coding Style
Running the coding standards fixer container

To check without applying any fixes, run the following command:
```
docker-compose run fixer fix --dry-run -v .
```
To fix all your PHP code to adhere the PSR2 Coding style, run:
```
docker-compose run fixer fix .
```
To apply fix only to a specific file
```
docker-compose run fixer fix <<file_name>>
```

## Unit Testing
PHPUnit
- Running a Test Case
```
docker-compose run php php artisan test
```
or you can dive inside the php container and execute the artisan command
```
docker-compose exec php bash; // enter the php container
php artisan test // run the laravel test
```

- Running the whole Test Suite
```
docker-compose run php php artisan --testsuite=<<directory>>
```

Read more in [laravel documentation for testing](https://laravel.com/docs/8.x/testing).

## Setting up ReactJS
Clone/Checkout/Paste ReactJS code inside `src/frontend`.
```
// Creates a react redux project inside `src/frontend` directory
docker-compose run node npx create-react-app . --template redux
```
If above command does not work, try deleting the `src/frontend` directory, then:
```
docker-compose run node npx create-react-app /var/www/frontend --template redux
```
For development, use command:
```
docker-compose run -p 3000:3000 -e CHOKIDAR_USEPOLLING=true npm run start
```
Access: `http://localhost:3000`.

For deployment, use command:
```
docker-compose run node npm run build
```
