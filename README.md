
![Dashboard](https://camo.githubusercontent.com/2a4760426be8e36619e0940538978a66e988811a/68747470733a2f2f696d672e7669727475626f782e6e65742f696d616765732f323031392f30392f32362f6235344d754c67694e662e676966)

### Hardware requirements
WordOps is very lightweight, it doesn't require a lot of resources and can be installed on low end devices like Raspberry PI. Minimum requirements are: ~100MB of storage and 512MB RAM
However, if you are going to use WordOps in production, some services like MySQL or Redis may need more resources, and running WordOps stacks without enough resources could impact your sites performance. Resources usage also highly depend on your site traffic.

Here our recommended hardware configuration for production: Multi-core CPU, 20GB SSD storage, 2GB RAM

[Hardware requirements](https://docs.wordops.net/getting-started/prerequesites/)

### Software requirements

The following operating systems are supported:

- [x] Ubuntu	20.04 LTS (focal)	x86_64
- [x] Ubuntu	18.04 LTS (bionic)	x86_64
- [x] Ubuntu	16.04 LTS (xenial)	x86_64
- [x] Debian	9 (stretch)	x86_64
- [x] Debian 10 (buster)	x86_64
- [x] Raspbian	9 (stretch)	armv7l
- [x] Raspbian	10 (buster)	armv7l

![ports](https://i.ibb.co/m89fxvj/Screenshot-2020-11-01-at-23-12-51.png)

### Login ssh

```
ssh root@IP_ADDRESS
YOUR_PSWD
```

### One-Step Automated Install
```
wget -qO wo wops.cc && sudo bash wo
```

Alternative: Clone Github repository and run

```
git clone https://github.com/WordOps/WordOps.git
cd WordOps/
sudo bash install
```

NOTE: if you're getting durring the installation the followind error `wo: line 860: wo: command not found` , then run the following command line:
```
sudo -H pip3 uninstall wordops && sudo -H pip3 install wordops" that should fix it up. And try again.
```
You should get the following answer:


*Welcome to WordOps install/update script v3.13.2*

*Installing wo dependencies	[OK]*
*Installing WordOps	[OK]*
*Running post-install steps	[OK]*
*WordOps (wo) require an username & and an email address to configure Git (used to save server configurations)*
*Your informations will ONLY be stored locally*
*Enter your name: `Your name`*
*Enter your email: `your email address`*
*Synchronizing wo database, please wait...*
*WordOps (wo) installed successfully*

*To enable bash-completion, just use the command:*
*`bash -l`*

*To install WordOps recommended stacks, you can use the command:*
*`wo stack install`*

*To create a first WordPress site, you can use the command:*
*`wo site create site.tld --wp`*

### Let's start
```
bash -l
wo stack install
```
You'll get ~ the following answers:

* WP-CLI is already installed* 
* Start : wo-kernel [OK]* 
* Adding repository for MySQL, please wait...* 
* Adding repository for NGINX, please wait...* 
* Adding repository for PHP, please wait...* 
* Updating apt-cache              [OK]
* Installing APT packages         [OK]
* Applying Nginx configuration templates
* Testing Nginx configuration     [OK]
* Restarting Nginx                [OK]
* Testing Nginx configuration     [OK]
* Restarting Nginx                [OK]
* Configuring php7.3-fpm
* Restarting php7.3-fpm           [OK]
* Tuning MySQL configuration      [OK] 
* Restarting mysql                [OK] 
* Restarting fail2ban             [OK] 
* Configuring Fail2Ban
* Configuring Sendmail            [OK] 
* Downloading PHPMyAdmin           [Done]
* Downloading phpRedisAdmin        [Done] 
* Downloading Composer             [Done]
* Downloading Adminer              [Done] 
* Downloading Adminer theme        [Done] 
* Downloading MySQLTuner           [Done] 
* Downloading Netdata              [Done] 
* Downloading WordOps Dashboard    [Done] 
* Downloading eXtplorer            [Done] 
* Downloading cheat.sh             [Done] 
* Downloading bash_completion      [Done] 
* Downloading clean.php            [Done] 
* Downloading opcache.php          [Done] 
* Downloading Opgui                [Done] 
* Downloading OCP.php              [Done] 
* Downloading Webgrind             [Done] 
* Downloading pt-query-advisor     [Done]
* Downloading Anemometer           [Done]
* Installing composer             [OK]
* Installing Netdata              [OK]
* Restarting netdata              [OK]
* Configuring packages            [OK]
* HTTP Auth User Name: `WordOps`
* HTTP Auth Password : `PASSWORD`
* WordOps backend is available on `https://IP_ADDRESS:22222` or `https://WEBSITE:22222`
* Successfully installed packages 


### create WordOps directories
```
mkdir -p /var/log/wo /var/lib/wo/tmp /var/lib/wo-backup
```


### Install acme.sh

clone the repository
```
git clone https://github.com/Neilpang/acme.sh.git /opt/acme.sh -q
```

create conf directory
```
mkdir -p /etc/letsencrypt/{config,live,renewal}
```

install acme.sh
```
cd /opt/acme.sh
./acme.sh --install
```

enable auto-upgrade
```
/etc/letsencrypt/acme.sh --config-home '/etc/letsencrypt/config' --upgrade --auto-upgrade
```

create .well-known directory
```
mkdir -p /var/www/html/.well-known/acme-challenge
```

set www-data as owner
```
chown -R www-data:www-data /var/www/html /var/www/html/.well-known
```

set permissions
```
chmod 750 /var/www/html /var/www/html/.well-known
```

After you'll create your website, run `sudo -E wo site update your_website_address --le --force` this will install the Let's Encrypt SSL.

### Let's create a website
```
wo site create your_website_address --wpfc
```

* Running pre-update checks       [OK]
* Setting up NGINX configuration 	[Done]
* Setting up webroot 		[Done]
* Downloading WordPress 		[Done]
* Setting up database		[Done]
* Configuring WordPress           [OK]
* Installing WordPress            [OK]
* Installing plugin nginx-helper  [OK]
* Testing Nginx configuration     [OK]
* Reloading Nginx                 [OK]
* WordPress admin user : `your_username`
* WordPress admin password : `your_password`
* Successfully created site `your_website_address`

### Allow remote access to MySQL
```
nano /etc/wo/wo.conf
```
and change the value for `grant-host` from localhost to `%` 

### TWEEKS WP-CONFIG 

```
define( 'WP_HOME', 'your_website_address' );
define( 'WP_SITEURL', 'your_website_address' );
define('WP_DEBUG', false);
define('WP_DEBUG_DISPLAY', false);
define('WP_MEMORY_LIMIT', '256M');
define('WP_MAX_MEMORY_LIMIT', '1024M');
define('CONCATENATE_SCRIPTS', false);
define('WP_POST_REVISIONS', '2');
define('MEDIA_TRASH', true);
define('EMPTY_TRASH_DAYS', '5');
define('WP_AUTO_UPDATE_CORE', 'minor');
define('FORCE_SSL_ADMIN', true);
define('DISALLOW_FILE_EDIT', true);
define('FS_METHOD', 'ssh2');
define('WP_AUTO_UPDATE_CORE', true);
add_filter('auto_update_plugin', '__return_false');
add_filter('auto_update_theme', '__return_false');
```

### Add extra MySQL username

```
mysql
SELECT User,Host FROM mysql.user;
CREATE USER 'your_username'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON *.* TO 'your_username'@'localhost';
```

### Install Grafana

To install the latest OSS release:

```
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

Add this repository for stable releases:

```
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

After you add the repository:

```
sudo apt-get update
sudo apt-get install grafana
```

Configure the Grafana server to start at boot:

```sudo systemctl enable grafana-server.service```

To start the service and verify that the service has started:

```
sudo service grafana-server start
sudo service grafana-server status
```

*LOGIN: http://your_ip_address:3000/login
admin / admin (but change the password at your first login*

