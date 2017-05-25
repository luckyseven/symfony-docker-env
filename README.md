# Symfony Dev Environment with Docker 

A ready-to-use environment for Symfony3 applications. You can use it also with Symfony2 applications with small changes.
Services included:

1. MySQL
2. MongoDB
3. Redis
4. PHP-FPM (v7)
5. ELK Stack (using `willdurand/elk` image)

## Installation

1. Copy `.env.dist` to `.env`.

    ```bash
    cp .env.dist .env
    ```
2. Open `.env` and edit variables:
    1. Set `SYMFONY_APP_PATH` with your Symfony source code's path.
    2. Set MySQL parameters (used in `docker-compose.yml`).
    You can add here more *Environment Variables* to use them in `docker-compose.yml`.

3. Build and run containers

    ```bash
    $ docker-compose build
    $ docker-compose up -d
    ```

4. Update your system host file
    
    ```bash
    $ sudo echo "127.0.0.1 symfony.dev" >> /etc/hosts
    ```
    **Note:** you can edit `nginx/main.conf` to change the default domain (`symfony.dev`)

5. Configure your Symfony parameters

    Update `app/config/parameters.yml`

    ```yml
    # parameters.yml
    parameters:
        database_host: mysql
        ...
    ```
    **Note:** Redis, MongoDB and MySQL containers will be recognized by PHP (and source code) container with hostnames from `docker-compose.yml`:
    * `mysql` is the default hostname for MySQL Container (default port `3306`)
    * `mongo` is the default hostname for MongoDB Container (default port `27017`)
    * `redis` is the default hostname for MySQL Container (default port `6379`)

6. Run provisioning commands from PHP container
    
    1. Open a shell using `docker-compose exec php bash`
    2. Run `composer install`
    3. Run Symfony commands (create databases, load fixtures...)
           
        ```bash   
        # Symfony3 console
        $ php bin/console doctrine:database:create
        # php bin/console ...
        ```
        **Note:** you can also use the alias `sf` instead of `php bin/console`:
        ```bash   
        # Symfony3 console alias
        $ sf doctrine:database:create
        # sf ...
        ```

## Usage

1. Run `docker-compose up -d` to enable:

    * Symfony app on port `8080`: [symfony.dev:8080](http://symfony.dev:8080)  
    * Symfony app in dev mode on port `8080`: [symfony.dev/app_dev.php:8080](http://symfony.dev/app_dev.php:8080)  
    * ELK Stack logs with Kibana on port `8081`: [symfony.dev:8081](http://symfony.dev:8081)
    * Nginx and Symfony logs in the host machine (inside `logs` directory)
2. Run `docker-compose down` to stop (and remove) containers created with `up`.

## Extra

**XDebug** already configured with IDE Key `PHPSTORM` on port `9001`


## Credits

* I was inspired by **maxopu**'s repository for this project. ([maxpou/docker-symfony](https://github.com/maxpou/docker-symfony)), mostly for the nginx container. 
* Also **m2sh**'s PHP7-FPM Dockerfile ([m2sh/php7](https://github.com/m2sh/php7)) was useful to complete my work.

## Contributing

Feel free to edit/update this repository or just fork it.
