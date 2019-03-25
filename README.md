Magento 2 Local Development Environment
=======================================

## 1. Overview

The purpose of this repo is to provide Magento 2 extensions developers an easy to setup local environment.

The setup was created using Docker, on top of the following images repository: https://github.com/thecodingmachine/docker-images-php (please check this repo for more info about technology stack, assumptions, etc.).

The setup was created on Ubuntu 18.04 and is tested on this OS, however the plan is to make it OS-agnostic.

In current state the setup includes two Magento versions: 2.1.16 and 2.2.6. They are both installed with sample data.

**Disclaimer:** In many places of this README file the following string is used as a placeholder for Magento version: `<VER>`. Please replace it with `21` for Magento 2.1.16 and `22` for Magento 2.2.6.

## 2. Prerequisites
      
In order to setup this environment you need Docker and Docker Compose installed on your machine.

## 3. Installation

1. Clone this repository.
2. Copy `.env.sample` file to `src/mage<VER>/.env`.
3. Customize `.env` files if you need.
4. CD to `src/mage<VER>` and run `docker-compose up` inside `src/mage<VER>`.
5. CD to project root and run `docker build docker/php-cli -t magento2env_php`.
6. Add `127.0.0.1 magento2.local` to your local hosts file (on Ubuntu: `/etc/hosts`).
7. Your Magento frontend will be accessible by the following URL: `https://magento2.local:80<VER>`.
8. Your Magento backend will be accessible by the following URL: `https://magento2.local:80<VER>/admin`. Admin credentials can be found in `.env` file.

## 4. Additional setup

### 4.1. SSL certificates

Import `docker/app/files/magento2root.pem` root certificate to your browsers so they don't complain about self-signed certificates.

If you need to access Magento URLs not only from your browsers (eg. CURL), import the root certificate to your OS.

### 4.2. PHPStorm

#### 4.2.1. Docker

1. Open Settings.
2. Go to "Build, Execution, Deployment" > "Docker".
3. Add new Docker server with default config.

![PHPStorm Docker](docs/phpstorm-docker.png)

#### 4.2.2. PHP

1. Open Settings.
2. Go to "Languages & Frameworks" > "PHP".
3. Set "PHP language level" to "7.1".
4. Create new remote PHP interpreter using Docker:
    * Use Docker server created in 4.2.1
    * Use `magento2env_php:latest` Docker image
    * Set "Debugger extension" to `/usr/local/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so`
 
![PHPStorm PHP](docs/phpstorm-php.png)

![PHPStorm PHP Interpreter](docs/phpstorm-php-interpreter.png)

#### 4.2.3. Xdebug

No configuration needed. See: https://www.jetbrains.com/help/phpstorm/zero-configuration-debugging.html

#### 4.2.4. PHPUnit

1. Open Settings.
2. Go to "Languages & Frameworks" > "PHP" > "Test Frameworks".
3. Add new config:
    * Use CLI interpreter created in 4.2.2
    * Use Composer autoloader with following path to script: `/opt/project/src/mage<VER>/vendor/autoload.php`
    * Set Default configuration file to `/opt/project/src/mage<VER>/dev/tests/unit/phpunit.xml.dist`

![PHPStorm PHPUnit](docs/phpstorm-phpunit.png)

### 4.3. composer.json

If you need to require some additional packages with Composer, you have to create `composer.local.json` file in Magento root directory. Its content will be merged with `composer.json` file.

The recommended way of installing Magento extensions with this environment is via Composer. `composer.json` is set up in a way that it will treat everything inside `src/modules/*/` as a package.

## 5. Common routines

### 5.1. Run already installed Magento

```
cd src/mage<VER>
docker-compose up
```

### 5.2. Go inside container (SSH like) 

First find `mage<VER>` container name by running `docker ps`. Then run `docker exec -it <container name> /bin/bash`, example: `docker exec -it magento2env_mage22_1_fcb38b06f0c6 /bin/bash`.

When you're inside, you act as `docker` user and you can perform any console command (also with `sudo`), eg. `bin/magento`.

### 5.3. Set up your Magento extension to be developed locally

1. GIT clone your extension to `src/modules/<VENDOR>/<NAME>`, eg. `src/modules/Orba/Payupl`.
2. Add your extension as a dependency to Composer, using `src/mage<VER>/composer.local.json` file (see: 4.3). Example:
    ```
    {
        "require": {
            "orba/magento2-payupl": "*"
        }
    }
    ```
3. Run `docker-compose up` (it runs `composer install` automatically).
4. Run `bin/magento setup:upgrade` inside container.

## 6. Planned enhancements

1. Local e-mails
2. Magento 2.3

## 7. Contribution

Please don't hesitate to create an issue if you found any bug.

Also, please suggest your enhancements either via an issue or a pull request.