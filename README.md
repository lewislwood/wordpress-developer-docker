# WordPress-Developer-Docker

Easy WordPress development with Docker and Docker Compose..
This include a plugin boilerPlate Plugin & Theme
Also the the site and databases are persistant

With this project you can quickly run the following:
Wordpress, WPCli, PHPMyAdmin or Admininer.
 [phpMyAdmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin/)
- [MySQL](https://hub.docker.com/_/mysql/)

MGitHub to clone:
$ git clone https://github.com/lewislwood/wordpress-developer-docker

Contents:

- [Requirements](#requirements)
- [Configuration](#configuration)
- [Installation](#installation)
- [Usage](#usage)

## Requirements

Make sure you have the latest versions of **Docker** and **Docker Compose** installed on your machine.

Clone this repository or copy the files from this repository into a new folder. In the **docker-compose.yml** file you may change the IP address (in case you run multiple containers) or the database from MySQL to MariaDB.

Make sure to [add your user to the `docker` group](https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user) when using Linux.

## Configuration

Copy the example environment into `.env`

```
cp env.example .env
```

Edit the `.env` file to change the default IP address, MySQL root password and WordPress database name.

## Installation

Open a terminal and `cd` to the folder in which `docker-compose.yml` is saved and run:

```
docker-compose up
```

This creates two new folders next to your `docker-compose.yml` file.

* `wp-data` – used to store and restore database dumps
* `wp-app` – the location of your WordPress application

The containers are now built and running. You should be able to access the WordPress installation with the configured IP in the browser address. By default it is `http://127.0.0.1`.

For convenience you may add a new entry into your hosts file.

## Usage

### Starting containers

You can start the containers with the `up` command in daemon mode (by adding `-d` as an argument) or by using the `start` command:

```
docker-compose start
```

### Stopping containers

```
docker-compose stop
```

### Removing containers

To stop and remove all the containers use the`down` command:

```
docker-compose down
```

Use `-v` if you need to remove the database volume which is used to persist the database:

```
docker-compose down -v
```

### Project from existing source

Copy the `docker-compose.yml` file into a new directory. In the directory you create two folders:

* `wp-data` – here you add the database dump
* `wp-app` – here you copy your existing WordPress code

You can now use the `up` command:

```
docker-compose up
```

This will create the containers and populate the database with the given dump. You may set your host entry and change it in the database, or you simply overwrite it in `wp-config.php` by adding:

```
define('WP_HOME','http://wp-app.local');
define('WP_SITEURL','http://wp-app.local');
```

### Creating database dumps

```
./export.sh
```

### Developing a Theme

Configure the volume to load the theme in the container in the `docker-compose.yml`:

```
volumes:
  - ./theme-name/trunk/:/var/www/html/wp-content/themes/theme-name
```

### Developing a Plugin

Configure the volume to load the plugin in the container in the `docker-compose.yml`:

```
volumes:
  - ./plugin-name/trunk/:/var/www/html/wp-content/plugins/plugin-name
```

### WP CLI

The docker compose configuration also provides a service for using the [WordPress CLI](https://developer.wordpress.org/cli/commands/).

Sample command to install WordPress:

```
docker-compose run --rm wpcli core install --url=http://localhost --title=test --admin_user=admin --admin_email=test@example.com
```

Or to list installed plugins:

```
docker-compose run --rm wpcli plugin list
```

For an easier usage you may consider adding an alias for the CLI:

```
alias wp="docker-compose run --rm wpcli"
```

This way you can use the CLI command above as follows:

```
wp plugin list

``

### phpMyAdmin

You can also visit `http://127.0.0.1:8080` to access phpMyAdmin after starting the containers.
The default username is `root`, and the password is the same as supplied in the `.env` file.


?
This version I prefer Adminer instead of PHPMyAdmin so I have given 2 versions of the YYML file.  Simply copy the one you want over the Docker-Compose.yml file to get the default.

I also modified the mysql for wp-data to be a persistent datasorce folder.

By both wp-app and wp-data being persistent folder you can simply copy the entire folder contents as a backup as well.  Although the export.sh should be sufficient if you already have backed up the wp-config.php file.  Also backup up your custom plugins, usually use Git repository management for them.

If for some reason you want to modify the contents or copy wp-app or wp-data.  Then this can be accomplished with the following:* ls -l * `# to list owner id and group id
* the user id and group id should be "www-data" and wp-data should be "999"
* run: sudo chown -R userid:userid wp-app
   **  Replace UserID with your Userid.  This should make you the ownere of all those files.  Now you can copy or modify them.
   * Now copy those files whereever you want.
   * Run $ sudo chown -R www-data:www-data wp-app `# to restore it back
   * $sudo chown -R 999:999 wp-data

   To create backup points simply run the following:
   * $ bash export.sh
     This will create a backup sql file in the wp-data folder.  



For full recovery you will need an .sql, wpconfig.php file.  If you had custom SQL files or istalled plugins, you will want at least one backup of your wp-content files tree.  And your custom plugin folders in the directory.  Your own docker-compose.yml file if you modified it from mine.
Then you will launch the container and then adminer or phpmyadmin and restore the sql to the database.
Copy your wpconfig.php over the one in the folder.
Now you can go to your website.  If you did everything correctly you will not be asked to install wordpress, just your site comes up.

Alternative is simply copy the entire contents and copy it back when restoring.  Simply change owner back and your back in business.


