# MASKOT - Frontend

## Overview

The Maskot Frontend handles subscription billing for all Maskot customers.
It also provides and entry point for Maskot Customers to their Clubs CMS.

# 26/04/21 Working versions
```sh
    // Make sure your local versions are the following
    php: 7.3
    composer: 1.10
    homestead: latest version (https://laravel.com/docs/8.x/homestead#updating-homestead)

    // You may check their versions like this
    $ php -v
    $ composer --version

    //If your php artisan commands are not working clear cache/files
    php artisan optimize:clear

    //If the problem is not fixed remove cache file and try again
    rm bootstrap/cache/config.php
    php artisan optimize:clear

    
    
```

```sh
    // If on maskot:deploy you get 
    // Make sure the VM php version is 7.3
    $ vagrant up
    $ vagrant ssh
    $ php -v (If it is not 7.3, continue with steps)
    $ php73 //or php7.3
    // Now you should be able to run the deploy
    $ cd maskot-backend
    $ php artisan maskot:deploy
```

```sh
    // Install php with brew and toggle versions
    brew install php@7.3
    brew unlink <current version> E.g. php@7.4
    brew link php@7.3
    // Restart terminal & Check version
    php -v
    // Unlimit memory (only for first time) (memory issues on composer install)
    // Check the current php init file path
    php --ini or php --init
    // Copy path and open
    E.g.
    open /usr/local/etc/php/7.3/php.ini
    // Edit memory_limit line to -1 (maximum)
    memory_limit = -1


    // 
```

```sh
    // After deployment you should run the following routes once
    // Uncomment the routes with the following comment at file routes/app.php
    TEMP NEED TO BE COMMENT WHEN LIVE IS GOOD
    // Run all the routes included in your browser to enable the permissions
    E.g. http://frontend.rodolfo.mirkdev.co:8000/maskot/admin/set/new
```

# Development
## Installation
```sh
    // Project Setup
    $ git clone --recursive git@github.com:mirkco/maskot-frontend.git .
    $ composer install
    $ npm install
    $ php artisan storage:link
```

Now all necessary files are downloaded, spin up the VM.
This package is equipped to run via [Homestead](https://laravel.com/docs/5.3/homestead)

```sh
    $ cp .env.homestead .env
    
    # NOTE: THE APP_KEY (generated below) MUST BE THE EXACT SAME ON FRONTEND AND BACKEND
    $ php artisan key:generate
    
    $ php vendor/bin/homestead make
    $ vagrant up
```

Finally add `127.0.0.1 frontend.maskot.dev` to your `/etc/hosts` file.

### Database Configuration
This project then requires at least 2 databases to operate, and development requires and additional 2 (making 4 in total):

Database 1: `maskot_frontend_dev` - The database for the Frontend (only required if you don't have an instance of the frontend already running)

Database 2: `team_maskot` - The initial/default Club all users are subscribed to

Database 3: `sample_1` - An additional testing Club

Database 4: `sample_2` - An additional testing Club

SSH into your environment.
Run the following custom artisan command to deploy everything:
```sh
    $ php artisan maskot:deploy
```

Note you can completely reset all databases with the following command
```sh
    $ php artisan maskot:reset
```


##### Note on Databases:
The Maskot (Customer Portal) and Club Schemas are stored in a seperate repo mirko/maskot-databases.
The repo is added as a submodule under `/database`. All schema updates should be committed to this repo.


## Usage

Using in development will depend on your VM choice.

Homestead - access via `192.168.10.10`

Docker - access via `maskot-frontend.dev`

## Tests
`maskot-frontend` uses Laravel Dusk as it's test framework.

To run the tests in homestead, you have to do some extra steps to install some missing requirements

To get `chromedriver` to run, install the the following dependencies in your homestead box:
```sh
    # SSH into your homestead VM, and run the following..

    # [First time only] Install everything thats needed
    $ bash homestead-dusk-install.sh

    # Run phpunit using the one provider by composer
    $ ./vendor/phpunit/phpunit/phpunit

```
