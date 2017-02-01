# Cloudwalk PHP Stack Docker

Cloudwalk PHP Stack Docker. It facilitate running **PHP** Apps on **Docker**.

>Use Docker first and learn about it later.


<a name="Supported-Containers"></a>
### Supported Software (Containers)

- **Database Engines:**
	- MySQL
	- MariaDB
- **Cache Engines:**
	- Redis
	- Memcached
- **PHP Servers:**
	- NGINX
	- Apache2
- **PHP Compilers:**
	- PHP-FPM
- **Message Queueing Systems:**
	- RabbitMQ
	- RabbitMQ Console
- **Tools:**
	- ElasticSearch
	- Workspace
		- PHP7-CLI
		- Composer
		- Git
		- Linuxbrew
		- Node
		- Gulp
		- SQLite
		- xDebug
		- Envoy
		- Deployer
		- Vim
		- Yarn

>If you can't find your Software, build it yourself and add it to this list. Contributions are welcomed :)



<a name="what-is-docker"></a>
### What is Docker?

[Docker](https://www.docker.com) is an open-source project that automates the deployment of applications inside software containers, by providing an additional layer of abstraction and automation of [operating-system-level virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) on Linux, Mac OS and Windows.


<a name="Requirements"></a>
## Requirements

- [Git](https://git-scm.com/downloads)
- [Docker](https://www.docker.com/products/docker/) `>= 1.12`






<a name="Installation"></a>
## Installation

Choose the setup the best suits your needs.

#### A) Setup for Single Project:
*(In case you want a Docker environment for each project)*

##### A.1) Setup environment in existing Project:
*(In case you already have a project, and you want to setup an environment to run it)*

1 - Clone this repository on your project root directory:

```bash
git submodule add https://github.com/cloudwalkph/cwd-php-docker.git
```
>If you are not already using Git for your PHP project, you can use `git clone` instead of `git submodule`.

Note: In this case the folder structure will be like this:

```
- project1
	- cwd-php-docker
- project2
	- cwd-php-docker
```

##### A.2) Setup environment first then create project:
*(In case you don't have a project, and you want to create your project inside the Docker environment)*

1 - Clone this repository anywhere on your machine:

```bash
git clone https://github.com/cloudwalkph/cwd-php-docker.git
```
Note: In this case the folder structure will be like this:

```
- projects
	- cwd-php-docker
	- myProject
```

2 - Edit the `docker-compose.yml` file to map to your project directory once you have it (example: `- ../myProject:/var/www`).

3 - Stop and re-run your docker-compose command for the changes to take place.

```
docker-compose stop && docker-compose up -d XXXX YYYY ZZZZ ....
```


#### B) Setup for Multiple Projects:

1 - Clone this repository anywhere on your machine:

```bash
git clone https://github.com/cloudwalkph/cwd-php-docker.git
```

2 - Edit the `docker-compose.yml` file to map to your projects directories:

```
    applications:
        image: tianon/true
        volumes:
            - ../project1/:/var/www/project1
            - ../project2/:/var/www/project2
```

3 - You can access all sites by visiting `http://localhost/project1/public` and `http://localhost/project2/public` but of course that's not very useful so let's setup nginx quickly.


4 - Go to `nginx/sites` and copy `sample.conf.example` to `project1.conf` then to `project2.conf`

5 - Open the `project1.conf` file and edit the `server_name` and the `root` as follow:

```
    server_name project1.dev;
    root /var/www/project1/public;
```
Do the same for each project `project2.conf`, `project3.conf`,...

6 - Add the domains to the **hosts** files.

```
127.0.0.1  project1.dev
```

7 - Create your project Databases. Right now you have to do it manually by entering your DB container, until we automate it soon.






<a name="Usage"></a>
## Usage

**Read Before starting:**

If you are using **Docker Toolbox** (VM), do one of the following:

- Upgrade to Docker [Native](https://www.docker.com/products/docker) for Mac/Windows (Recommended).

<br>

1 - Run Containers: *(Make sure you are in the `cwd-php-docker` folder before running the `docker-compose` commands).*



**Example:** Running NGINX and MySQL:

```bash
docker-compose up -d nginx mysql
```

**Note**: The `workspace` and `php-fpm` will run automatically in most of the cases, so no need to specify them in the `up` command. If you couldn't find them running then you need specify them as follow: `docker-compose up -d nginx php-fpm mysql workspace`.


You can select your own combination of Containers form the list below:

`nginx`, `php-fpm`, `mysql`, `redis`, `mariadb`, `apache2`, `memcached`, `rabbitmq`, `workspace`, `elasticsearch`.


<br>
2 - Enter the Workspace container, to execute commands like (Artisan, Composer, PHPUnit, Gulp, ...).

```bash
docker-compose exec workspace bash
```

Alternativey, for Windows Powershell users: execute the following command to enter any running container:

```bash
docker exec -it {workspace-container-id} bash
```

**Note:** You can add `--user=laradock` (example `docker-compose exec --user=laradock workspace bash`) to have files created as your host's user. (you can change the PUID (User id) and PGID (group id) variables from the `docker-compose.yml`).

<br>
3 - Edit your project configurations.

Open your `.env` file and set the `DB_HOST` to `mysql`:

```env
DB_HOST=mysql
```

*If you want to use Laravel and you don't have it installed yet, see [How to Install Laravel in a Docker Container](#Install-Laravel).*

<br>
4 - Open your browser and visit your localhost address (`http://localhost/`).

<br>
**Debugging**: if you are facing any problem here check the [Debugging](#debugging) section.







<br>
<a name="Documentation"></a>
## Documentation






<a name="Docker"></a>






<a name="List-current-running-Containers"></a>
### List current running Containers
```bash
docker ps
```
You can also use the following command if you want to see only this project containers:

```bash
docker-compose ps
```






<br>
<a name="Close-all-running-Containers"></a>
### Close all running Containers
```bash
docker-compose stop
```

To stop single container do:

```bash
docker-compose stop {container-name}
```






<br>
<a name="Delete-all-existing-Containers"></a>
### Delete all existing Containers
```bash
docker-compose down
```






<br>
<a name="Enter-Container"></a>
### Enter a Container (run commands in a running Container)

1 - First list the current running containers with `docker ps`

2 - Enter any container using:

```bash
docker-compose exec {container-name} bash
```

*Example: enter MySQL container*

```bash
docker-compose exec mysql bash
```

3 - To exit a container, type `exit`.






<br>
<a name="Edit-Container"></a>
### Edit default container configuration
Open the `docker-compose.yml` and change anything you want.

Examples:

Change MySQL Database Name:

```yml
    environment:
        MYSQL_DATABASE: medix
    ...
```

Change Redis defaut port to 1111:

```yml
    ports:
        - "1111:6379"
    ...
```






<br>
<a name="Edit-a-Docker-Image"></a>
### Edit a Docker Image

1 - Find the `dockerfile` of the image you want to edit,
<br>
example for `mysql` it will be `mysql/Dockerfile`.

2 - Edit the file the way you want.

3 - Re-build the container:

```bash
docker-compose build mysql
```
More info on Containers rebuilding [here](#Build-Re-build-Containers).






<br>
<a name="Build-Re-build-Containers"></a>
### Build/Re-build Containers

If you do any change to any `dockerfile` make sure you run this command, for the changes to take effect:

```bash
docker-compose build
```
Optionally you can specify which container to rebuild (instead of rebuilding all the containers):

```bash
docker-compose build {container-name}
```

You might use the `--no-cache` option if you want full rebuilding (`docker-compose build --no-cache {container-name}`).








<br>
<a name="View-the-Log-files"></a>
### View the Log files
The Nginx Log file is stored in the `logs/nginx` directory.

However to view the logs of all the other containers (MySQL, PHP-FPM,...) you can run this:

```bash
docker logs {container-name}
```






<br>
<a name="PHP"></a>






<a name="Install-PHP-Extensions"></a>
### Install PHP Extensions

Before installing PHP extensions, you have to decide whether you need for the `FPM` or `CLI` because each lives on a different container, if you need it for both you have to edit both containers.

The PHP-FPM extensions should be installed in `php-fpm/Dockerfile-XX`. *(replace XX with your default PHP version number)*.
<br>
The PHP-CLI extensions should be installed in `workspace/Dockerfile`.






<br>
<a name="Change-the-PHP-FPM-Version"></a>
### Change the (PHP-FPM) Version
By default **PHP-FPM 7.0** is running.

>The PHP-FPM is responsible of serving your application code, you don't have to change the PHP-CLI version if you are planning to run your application on different PHP-FPM version.


#### A) Switch from PHP `7.0` to PHP `5.6`

1 - Open the `docker-compose.yml`.

2 - Search for `Dockerfile-70` in the PHP container section.

3 - Change the version number, by replacing `Dockerfile-70` with `Dockerfile-56`, like this:

```yml
    php-fpm:
        build:
            context: ./php-fpm
            dockerfile: Dockerfile-70
    ...
```

4 - Finally rebuild the container

```bash
docker-compose build php-fpm
```

> For more details about the PHP base image, visit the [official PHP docker images](https://hub.docker.com/_/php/).


#### B) Switch from PHP `7.0` or `5.6` to PHP `5.5`
<br>
<a name="Change-the-PHP-CLI-Version"></a>
### Change the PHP-CLI Version
By default **PHP-CLI 7.0** is running.

>Note: it's not very essential to edit the PHP-CLI version. The PHP-CLI is only used for the Artisan Commands & Composer. It doesn't serve your Application code, this is the PHP-FPM job.

The PHP-CLI is installed in the Workspace container. To change the PHP-CLI version you need to edit the `workspace/Dockerfile`.

Right now you have to manually edit the `Dockerfile` or create a new one like it's done for the PHP-FPM. (consider contributing).






<br>
<a name="Install-xDebug"></a>
### Install xDebug

1 - First install `xDebug` in the Workspace and the PHP-FPM Containers:
<br>
a) open the `docker-compose.yml` file
<br>
b) search for the `INSTALL_XDEBUG` argument under the Workspace Container
<br>
c) set it to `true`
<br>
d) search for the `INSTALL_XDEBUG` argument under the PHP-FPM Container
<br>
e) set it to `true`

It should be like this:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - INSTALL_XDEBUG=true
    ...
    php-fpm:
        build:
            context: ./php-fpm
            args:
                - INSTALL_XDEBUG=true
    ...
```

2 - Re-build the containers `docker-compose build workspace php-fpm`

3 - Open `cwd-php-docker/workspace/xdebug.ini` and/or `cwd-php-docker/php-fpm/xdebug.ini` and enable at least the following configs:

```
xdebug.remote_autostart=1
xdebug.remote_enable=1
xdebug.remote_connect_back=1
```






<br>
<a name="Control-xDebug"></a>
### Start/Stop xDebug:

By installing xDebug, you are enabling it to run on startup by default.

To control the behavior of xDebug (in the `php-fpm` Container), you can run the following commands from the cwd-php-docker root folder, (at the same prompt where you run docker-compose):

- Stop xDebug from running by default: `./xdebugPhpFpm stop`.
- Start xDebug by default: `./xdebugPhpFpm start`.
- See the status: `./xdebugPhpFpm status`.






<br>
<a name="Production"></a>






<br>
<a name="LaraDock-for-Production"></a>
### Prepare LaraDock for Production

It's recommended for production to create a custom `docker-compose.yml` file. For that reason, LaraDock is shipped with `production-docker-compose.yml` which should contain only the containers you are planning to run on production (usage exampe: `docker-compose -f production-docker-compose.yml up -d nginx mysql redis ...`).

Note: The Database (MySQL/MariaDB/...) ports should not be forwarded on production, because Docker will automatically publish the port on the host, which is quite insecure, unless specifically told not to. So make sure to remove these lines:

```
ports:
    - "3306:3306"
```

To learn more about how Docker publishes ports, please read [this excellent post on the subject](https://fralef.me/docker-and-iptables.html).






<br>
<a name="Digital-Ocean"></a>
### Setup Laravel and Docker on Digital Ocean

####[Full Guide Here](https://github.com/LaraDock/laradock/blob/master/_guides/digital_ocean.md)






<br>
<a name="Laravel"></a>






<a name="Install-Laravel"></a>
### Install Laravel from a Docker Container

1 - First you need to enter the Workspace Container.

2 - Install Laravel.

Example using Composer

```bash
composer create-project laravel/laravel my-cool-app "5.2.*"
```

> We recommend using `composer create-project` instead of the Laravel installer, to install Laravel.

For more about the Laravel installation click [here](https://laravel.com/docs/master#installing-laravel).


3 - Edit `docker-compose.yml` to Map the new application path:

By default, cwd-php-docker assumes the Laravel application is living in the parent directory of the cwd-php-docker folder.

Since the new Laravel application is in the `my-cool-app` folder, we need to replace `../:/var/www` with `../my-cool-app/:/var/www`, as follow:

```yaml
    application:
		 image: tianon/true
        volumes:
            - ../my-cool-app/:/var/www
    ...
```
4 - Go to that folder and start working..

```bash
cd my-cool-app
```

5 - Go back to the LaraDock installation steps to see how to edit the `.env` file.






<br>
<a name="Run-Artisan-Commands"></a>
### Run Artisan Commands

You can run artisan commands and many other Terminal commands from the Workspace container.

1 - Make sure you have the workspace container running.

```bash
docker-compose up -d workspace // ..and all your other containers
```

2 - Find the Workspace container name:

```bash
docker-compose ps
```

3 - Enter the Workspace container:

```bash
docker-compose exec workspace bash
```

Add `--user=laradock` (example `docker-compose exec --user=laradock workspace bash`) to have files created as your host's user.


4 - Run anything you want :)

```bash
php artisan
```
```bash
Composer update
```
```bash
phpunit
```






<br>
<a name="Use-Redis"></a>
### Use Redis

1 - First make sure you run the Redis Container (`redis`) with the `docker-compose up` command.

```bash
docker-compose up -d redis
```

2 - Open your Laravel's `.env` file and set the `REDIS_HOST` to `redis`

```env
REDIS_HOST=redis
```

If you don't find the `REDIS_HOST` variable in your `.env` file. Go to the database configuration file `config/database.php` and replace the default `127.0.0.1` IP with `redis` for Redis like this:

```php
'redis' => [
    'cluster' => false,
    'default' => [
        'host'     => 'redis',
        'port'     => 6379,
        'database' => 0,
    ],
],
```

3 - To enable Redis Caching and/or for Sessions Management. Also from the `.env` file set `CACHE_DRIVER` and `SESSION_DRIVER` to `redis` instead of the default `file`.

```env
CACHE_DRIVER=redis
SESSION_DRIVER=redis
```

4 - Finally make sure you have the `predis/predis` package `(~1.0)` installed via Composer:

```bash
composer require predis/predis:^1.0
```

5 - You can manually test it from Laravel with this code:

```php
\Cache::store('redis')->put('LaraDock', 'Awesome', 10);
```


<br>
<a name="Use-ElasticSearch"></a>
### Use ElasticSearch

1 - Run the ElasticSearch Container (`elasticsearch`) with the `docker-compose up` command:

```bash
docker-compose up -d elasticsearch
```

2 - Open your browser and visit the localhost on port **9200**:  `http://localhost:9200`


#### Install ElasticSearch Plugin

1 - Install the ElasticSearch plugin like [delete-by-query](https://www.elastic.co/guide/en/elasticsearch/plugins/current/plugins-delete-by-query.html).

```bash
docker exec {container-name} /usr/share/elasticsearch/bin/plugin install delete-by-query
```

2 - Restart elasticsearch container

```bash
docker restart {container-name}
```



<br>
<a name="Change-the-timezone"></a>
### Change the timezone

To change the timezone for the `workspace` container, modify the `TZ` build argument in the Docker Compose file to one in the [TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

For example, if I want the timezone to be `New York`:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - TZ=America/New_York
    ...
```

We also recommend [setting the timezone in Laravel](http://www.camroncade.com/managing-timezones-with-laravel/).






<br>
<a name="CronJobs"></a>
### Adding cron jobs

You can add your cron jobs to `workspace/crontab/root` after the `php artisan` line.

```
* * * * * php /var/www/artisan schedule:run >> /dev/null 2>&1

# Custom cron
* * * * * root echo "Every Minute" > /var/log/cron.log 2>&1
```

Make sure you [change the timezone](#Change-the-timezone) if you don't want to use the default (UTC).






<br>
<a name="Workspace-ssh"></a>
### Access workspace via ssh

You can access the `workspace` container through `localhost:2222` by setting the `INSTALL_WORKSPACE_SSH` build argument to `true`.

To change the default forwarded port for ssh:

```yml
    workspace:
		ports:
			- "2222:22" # Edit this line
    ...
```






<br>
<a name="MySQL-access-from-host"></a>
### MySQL access from host

You can forward the MySQL/MariaDB port to your host by making sure these lines are added to the `mysql` or `mariadb` section of the `docker-compose.yml` or in your [environment specific Compose](https://docs.docker.com/compose/extends/) file.

```
ports:
    - "3307:3306"
```






<br>
<a name="MySQL-root-access"></a>
### MySQL root access

The default username and password for the root mysql user are `root` and `root `.

1 - Enter the mysql contaier: `docker-compose exec mysql bash`.

2 - Enter mysql: `mysql -uroot -proot` for non root access use `mysql -uhomestead -psecret`.

3 - See all users: `SELECT User FROM mysql.user;`

4 - Run any commands `show databases`, `show tables`, `select * from.....`.






<br>
<a name="Change-MySQL-port"></a>
### Change MySQL port

Modify the `mysql/my.cnf` file to set your port number, `1234` is used as an example.

```
[mysqld]
port=1234
```

If you need <a href="#MySQL-access-from-host">MySQL access from your host</a>, do not forget to change the internal port number (`"3306:3306"` -> `"3306:1234"`) in the docker-compose configuration file.






<br>
<a name="Use-custom-Domain"></a>
### Use custom Domain (instead of the Docker IP)

Assuming your custom domain is `laravel.dev`

1 - Open your `/etc/hosts` file and map your localhost address `127.0.0.1` to the `laravel.dev` domain, by adding the following:

```bash
127.0.0.1    laravel.dev
```

2 - Open your browser and visit `{http://laravel.dev}`


Optionally you can define the server name in the nginx configuration file, like this:

```conf
server_name laravel.dev;
```






<br>
<a name="Enable-Global-Composer-Build-Install"></a>
### Enable Global Composer Build Install

Enabling Global Composer Install during the build for the container allows you to get your composer requirements installed and available in the container after the build is done.

1 - Open the `docker-compose.yml` file

2 - Search for the `COMPOSER_GLOBAL_INSTALL` argument under the Workspace Container and set it to `true`

It should be like this:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - COMPOSER_GLOBAL_INSTALL=true
    ...
```
3 - Now add your dependencies to `workspace/composer.json`

4 - Re-build the Workspace Container `docker-compose build workspace`






<br>
<a name="Install-Prestissimo"></a>
### Install Prestissimo

[Prestissimo](https://github.com/hirak/prestissimo) is a plugin for composer which enables parallel install functionality.

1 - Enable Running Global Composer Install during the Build:

Click on this [Enable Global Composer Build Install](#Enable-Global-Composer-Build-Install) and do steps 1 and 2 only then continue here.

2 - Add prestissimo as requirement in Composer:

a - Now open the `workspace/composer.json` file

b - Add `"hirak/prestissimo": "^0.3"` as requirement

c - Re-build the Workspace Container `docker-compose build workspace`






<br>
<a name="Install-Node"></a>
### Install Node + NVM

To install NVM and NodeJS in the Workspace container

1 - Open the `docker-compose.yml` file

2 - Search for the `INSTALL_NODE` argument under the Workspace Container and set it to `true`

It should be like this:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - INSTALL_NODE=true
    ...
```

3 - Re-build the container `docker-compose build workspace`






<br>
<a name="Install-Yarn"></a>
### Install Node + YARN

Yarn is a new package manager for JavaScript. It is so faster than npm, which you can find [here](http://yarnpkg.com/en/compare).To install NodeJS and [Yarn](https://yarnpkg.com/) in the Workspace container:

1 - Open the `docker-compose.yml` file

2 - Search for the `INSTALL_NODE` and `INSTALL_YARN` argument under the Workspace Container and set it to `true`

It should be like this:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - INSTALL_NODE=true
                - INSTALL_YARN=true
    ...
```

3 - Re-build the container `docker-compose build workspace`






<br>
<a name="Install-Linuxbrew"></a>
### Install Linuxbrew

Linuxbrew is a package manager for Linux. It is the Linux version of MacOS Homebrew and can be found [here](http://linuxbrew.sh). To install Linuxbrew in the Workspace container:

1 - Open the `docker-compose.yml` file

2 - Search for the `INSTALL_LINUXBREW` argument under the Workspace Container and set it to `true`

It should be like this:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - INSTALL_LINUXBREW=true
    ...
```

3 - Re-build the container `docker-compose build workspace`





<br>
<a name="Common-Aliases"></a>
<br>
### Common Terminal Aliases
When you start your docker container, cwd-php-docker will copy the `aliases.sh` file located in the `cwd-php-docker/workspace` directory and add sourcing to the container `~/.bashrc` file.

You are free to modify the `aliases.sh` as you see fit, adding your own aliases (or function macros) to suit your requirements.



#### I see a blank (white) page instead of the Laravel 'Welcome' page!

Run the following command from the Laravel root directory:

```bash
sudo chmod -R 777 storage bootstrap/cache
```






#### I see "Welcome to nginx" instead of the Laravel App!

Use `http://127.0.0.1:8000` instead of `http://localhost:8000` in your browser.






#### I see an error message containing `address already in use` or `port is already allocated`

Make sure the ports for the services that you are trying to run (22, 8000, 443, 3306, etc.) are not being used already by other programs on the host, such as a built in `apache`/`httpd` service or other development tools you have installed.






#### I get Nginx error 404 Not Found on Windows.

1. Go to docker Settings on your Windows machine.
2. Click on the `Shared Drives` tab and check the drive that contains your project files.
3. Enter your windows username and password.
4. Go to the `reset` tab and click restart docker.




#### The time in my services does not match the current time

1. Make sure you've [changed the timezone](#Change-the-timezone).
2. Stop and rebuild the containers (`docker-compose up -d --build <services>`)




#### I get Mysql connection refused

This error sometimes happens because your Laravel application isn't running on the container localhost IP (Which is 127.0.0.1). Steps to fix it:

* Option A
  1. Check your running Laravel application IP by dumping `Request::ip()` variable using `dd(Request::ip())` anywhere on your application. The result is the IP of your Laravel container.
  2. Change the `DB_HOST` variable on env with the IP that you received from previous step.
* Option B
   1. Change the `DB_HOST` value to the same name as the mysql docker container. The cwd-php-docker docker-compose file currently has this as `mysql`
