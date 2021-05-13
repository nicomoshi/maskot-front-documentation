# MASKOT - Frontend

## Table of Contents
- [MASKOT - Frontend](#maskot---frontend)
  * [Before you install](#before-you-install)
    + [Requirements](#requirements)
  * [Install Repo & Packages](#install-repo---packages)
    + [1. Clone Repository:](#1-clone-repository-)
    + [2. Install NPM Packages:](#2-install-npm-packages-)
    + [3. Install Composer Packages:](#3-install-composer-packages-)
    + [4. Link storage:](#4-link-storage-)
  * [Prepare VM](#prepare-vm)
    + [1. Create environment file:](#1-create-environment-file-)
    + [2. Edit environment file:](#2-edit-environment-file-)
    + [3. Make Homestead file:](#3-make-homestead-file-)
    + [4. Edit Homestead.yaml:](#4-edit-homesteadyaml-)
    + [5. Save hosts on Computer (OSX):](#5-save-hosts-on-computer--osx--)
    + [6. Start VM:](#6-start-vm-)
    + [7. Change VM PHP & Composer version (Different to local):](#7-change-vm-php---composer-version--different-to-local--)
- [Run Maskot](#run-maskot)
    + [1. Run Migrations:](#1-run-migrations-)
    + [2. Access Front-end:](#2-access-front-end-)
    + [3. Before registering, set up permissions:](#3-before-registering--set-up-permissions-)
    + [4. Before sign in, validate user & add backend path to your hosts:](#4-before-sign-in--validate-user---add-backend-path-to-your-hosts-)

## Before you install

### Requirements


[Full Requirements Page](https://www.notion.so/Maskot-da6fb6a910b840fe8e8ebbe2bf0c39b3)

- [Homebrew](https://www.notion.so/HomeBrew-b1cc1dbaa3db4a148a07953c0b772ad7) Latest
- [PHP](https://www.notion.so/PHP-60b5ef2e2bde4d559c8dc27714a0a326) 7.3
- [NPM](https://www.notion.so/NPM-20b8a81d6243444f8ef844ec0655ab0a) Latest
- [Vagrant](https://www.notion.so/Vagrant-653a00de2f9b4991a9832f3058545f39) Latest
- [VirtualBox](https://www.notion.so/VirtualBox-a8a846d5b1374be39cb09df29130fed8) Latest
- [Homestead](https://www.notion.so/Homestead-e4bf957a632f433b925d432b9e9f453e) Latest
- [Composer](https://www.notion.so/Composer-63392bc980454f05ad0a9d37fdbe897c) 1.10.1
- [Sequel Pro](https://www.notion.so/Sequel-Pro-ebb9691c34134109ba5c50d3a6c2c309)
- .env file (Ask Developer)

## Install Repo & Packages

### 1. Clone Repository:
```sh
git clone --recursive git@github.com:mirkco/maskot-frontend.git .
```

### 2. Install NPM Packages:
```sh
npm install
```

### 3. Install Composer Packages:
```sh
composer install
```

### 4. Link storage:
```sh
php artisan storage:link
```

## Prepare VM

### 1. Create environment file:
```sh
cp .env.homestead .env
```

### 2. Edit environment file:
In project's root folder, replace .env with file provided by developer.

### 3. Make Homestead file:
```sh
php vendor/bin/homestead make
```

### 4. Edit Homestead.yaml:
**1.** Locate Homestead.yaml file in project't root folder, replace with:
```sh
ip: 192.168.10.10
memory: 2048
cpus: 1
hostname: maskot-frontend
name: maskot-frontend
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
  - ~/.ssh/id_rsa
folders:
  - map: [localPath]/maskot-frontend
    to: /home/vagrant/maskot-frontend
  - map: [localPath]/maskot-backend
    to: /home/vagrant/maskot-backend
sites:
  - map: frontend.default.mirkdev.co
    to: /home/vagrant/maskot-frontend/public
  - map: default.default.mirkdev.co
    to: /home/vagrant/maskot-backend/public
databases:
  - homestead
  - maskot_frontend_dev
  - team_maskot

```
**2.** Replace **[localPath]** with local project's path (E.g. /Users/Developer/Projects/maskot-frontend)

```diff
- Before next steps follow maskot-backend installation
```
[maskot-backend](https://gitlab.com/maskot.co/maskot-backend)

### 5. Save hosts on Computer (OSX):
**1.** Get your local IP:
System Preferences -> Network -> Advanced -> TCP/IP -> IPv4 Address = [localIP]

**2.** Open your local hosts file:
```sh
sudo nano /etc/hosts
```
**3.** Add hosts as localIP (Use arrows to navigate and add hosts under your current hosts):
```sh
[localIP] frontend.default.mirkdev.co
[localIP] default.default.mirkdev.co
```

**4.** Save hosts:
**Ctrl + X** -> **Y** -> **Enter**

### 6. Start VM:
```sh
vagrant up
```

### 7. Change VM PHP & Composer version (Different to local):
**1.** Access ssh
```sh
vagrant ssh
```
**2.** Check php version
```sh
php -v
```
**3.** If php version is > 7.3
```sh
php7.3
# or
php73

# check php version again.
# if nothing is displayed, reopen terminal, access ssh again & check php version.
```

**4.** Check composer version
```sh
composer -V
```

**5.** If composer version > 1.10.1
```sh
composer selfupdate 1.10.1
# check composer version again
```

# Run Maskot

### 1. Run Migrations:

```sh
vagrant up
vagrant ssh
cd maskot-backend
php artisan maskot:deploy --force
```

### 2. Access Front-end:
In Browser: http://frontend.default.mirkdev.co:8000

### 3. Before registering, set up permissions:
**1.** Uncomment in routes/web.php:
```sh
// TEMP NEED TO BE COMMENT WHEN LIVE IS GOOD
//Route::get('/maskot/admin/set/new', 'FrontendController@setInitialRolesAndPermission');
//Route::get('/maskot/admin/set/additional', 'FrontendController@setAdditionalRolesAndPermission');
//Route::get('/maskot/admin/set/link', 'FrontendController@linkAppUserToFullAccess');
//Route::get('/maskot/admin/set/username', 'FrontendController@emailToUsername');
//Route::get('/maskot/admin/set/maskot/subscribe', 'FrontendController@subscribeAllUserToMaskot');
// END -------------------------------------

```
**2.** Save & Run routes in browser:

http://frontend.default.mirkdev.co:8000/maskot/admin/set/new

http://frontend.default.mirkdev.co:8000/maskot/admin/set/additional

http://frontend.default.mirkdev.co:8000/maskot/admin/set/link

http://frontend.default.mirkdev.co:8000/maskot/admin/set/username

http://frontend.default.mirkdev.co:8000/maskot/admin/set/maskot/subscribe

**3.** Comment routes again and you are ready!

### 4. Before sign in, validate user & add backend path to your hosts:
**1.** Open Sequel Pro or your database manager, connect to database:
```sh
Type: Standard
Name: database_maskot_local
Host: 127.0.0.1
Username: homestead
Password: secret
Port: 33060
```
**2.** Go to maskot_frontend_dev database, in maskot_admin_users_on_clubs & maskot_app_users_on_clubs tables, change active value on new user:
```sh
active = 1
```
**3.** Try to login, when getting DNS not found error, copy that address and add to your hosts:
E.g. etc/hosts/
```sh
[localIP] v0jb5s63.default.mirkdev.co
```

**4.** Login again and Enjoy Maskot!


