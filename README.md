This provides a docker-based development environment to start working with Laravel.

### Install Docker

[Get Docker](https://docs.docker.com/get-docker/)

### App setup

```sh
cp .env.example .env
docker-compose up
docker-compose exec app composer install
docker-compose exec app php artisan key:generate
docker-compose exec app php artisan migrate
docker-compose exec front npm install && npm run dev
```

### App

The app should be available via http://localhost:8000/.

### MySQL

You can easily import a data dump into MySQL if you'd like to. The MySQL container [will automatically](https://hub.docker.com/_/mysql#:~:text=Initializing%20a%20fresh%20instance) 
will detect a dump in `.docker/mysql/config/initdb` directory, unpack and import it.

However, if you have already initialized the db container, then you will need to remove the data before import: 
```sh
docker-compose down
rm -rf .docker/mysql/data
docker-compose up
```

You can connect to the DB from your host machine through `33060` port. 

-----

### How to use in existing Laravel project ?

Simple, just copy the following files/folders into the existing project and adjust the existing `.env` file with the values of the provided `.env.example`.

- `.docker`
- `.env.example`
- `docker-compose.yml`