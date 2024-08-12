# SuiteCRM
SuiteCRM is an open-source CRM software that offers robust tools for managing customer relationships, sales, marketing, and support. It is highly customizable and scalable, providing cost-effective solutions for businesses to enhance customer engagement and streamline operations. 



<a href="https://suitecrm.com">
  <img width="180px" height="41px" src="https://suitecrm.com/wp-content/uploads/2017/12/logo.png" align="right" />
</a>

# SuiteCRM 7.14.4

[![Build Status](https://travis-ci.org/salesagility/SuiteCRM.svg?branch=hotfix)](https://travis-ci.org/salesagility/SuiteCRM)
[![codecov](https://codecov.io/gh/salesagility/SuiteCRM/branch/hotfix/graph/badge.svg)](https://codecov.io/gh/salesagility/SuiteCRM/branch/hotfix)
[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/suitecrm/Lobby)
[![LICENSE](https://img.shields.io/github/license/suitecrm/suitecrm.svg)](https://github.com/salesagility/suitecrm/blob/hotfix/LICENSE.txt)
[![GitHub contributors](https://img.shields.io/github/contributors/salesagility/suitecrm)](https://github.com/salesagility/SuiteCRM/graphs/contributors)
[![Twitter](https://img.shields.io/twitter/follow/suitecrm.svg?style=social&label=Follow)](https://twitter.com/intent/follow?screen_name=suitecrm)

[Website](https://suitecrm.com) | 
[Demo](https://suitecrm.com/demo/) |
[Maintainers](https://salesagility.com) |
[Contributors](https://github.com/salesagility/SuiteCRM/graphs/contributors) |
[Community & Forum](https://suitecrm.com/suitecrm/forum) |
[Partners](https://suitecrm.com/about/about-us/partners/) |
[Extensions Directory](https://store.suitecrm.com/) |
[Translations](https://crowdin.com/project/suitecrmtranslations) | [Code of Conduct](https://docs.suitecrm.com/community/code-of-conduct/)

<img src="(https://f.hellowork.com/bdmtools/2022/10/suitecrm-outil-1.jpg)"  />

[SuiteCRM](https://suitecrm.com) is the award-winning open-source, enterprise-ready Customer Relationship Management (CRM) software application.

Our vision is to be the most adopted open source enterprise CRM in the world, giving users full control of their data and freedom to own and customise their business solution.

Try out a free fully working [SuiteCRM demo available here](https://suitecrm.com/demo/)

##Installation##

To install SuiteCRM using Nginx, MySQL, and an SSL certificate on Ubuntu 22.04, follow these steps:

### 1. Update and Install Required Packages

First, update your package list and install necessary packages:

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx mysql-server php-fpm php-mysql php-xml php-mbstring php-curl php-zip unzip git
```

### 2. Configure MySQL

Secure your MySQL installation and create a database for SuiteCRM:

```bash
sudo mysql_secure_installation
```

Log in to MySQL to create a database and user for SuiteCRM:

```bash
sudo mysql -u root -p
```

Execute the following commands in the MySQL shell:

```sql
CREATE DATABASE suitecrm_db;
CREATE USER 'suitecrm_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON suitecrm_db.* TO 'suitecrm_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 3. Download and Install SuiteCRM

Download SuiteCRM and extract it to your web directory:

```bash
cd /var/www/
sudo wget https://suitecrm.com/files/161/SuiteCRM-8.3/519/SuiteCRM-8.3.0.zip
sudo unzip SuiteCRM-8.3.0.zip
sudo mv SuiteCRM-8.3.0 suitecrm
```

Change ownership of the SuiteCRM directory:

```bash
sudo chown -R www-data:www-data /var/www/suitecrm
sudo chmod -R 755 /var/www/suitecrm
```

### 4. Configure PHP

Ensure the PHP configuration is suitable for SuiteCRM. Edit `/etc/php/8.1/fpm/php.ini` (adjust the PHP version if needed):

```bash
sudo nano /etc/php/8.1/fpm/php.ini
```

Adjust or add the following settings:

```ini
memory_limit = 512M
upload_max_filesize = 50M
post_max_size = 50M
max_execution_time = 300
```

Restart PHP-FPM:

```bash
sudo systemctl restart php8.1-fpm
```

### 5. Configure Nginx

Create a new Nginx server block for SuiteCRM:

```bash
sudo nano /etc/nginx/sites-available/suitecrm
```

Add the following configuration:

```nginx
server {
    listen 80;
    server_name your_domain.com;

    root /var/www/suitecrm;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Enable the new server block and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/suitecrm /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### 6. Install SSL Certificate

You can use Certbot to obtain a free SSL certificate from Let’s Encrypt:

```bash
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your_domain.com
```

Follow the prompts to configure SSL.

### 7. Complete SuiteCRM Installation

Visit your domain (e.g., `https://your_domain.com`) in a web browser to start the SuiteCRM installation process. Follow the on-screen instructions and provide the database details created earlier.

### 8. Finalize Setup

After installation, secure your SuiteCRM by updating permissions and removing the installation directory:

```bash
sudo rm -rf /var/www/suitecrm/install
sudo chown -R www-data:www-data /var/www/suitecrm
```

Your SuiteCRM installation should now be up and running with Nginx, MySQL, and SSL on Ubuntu 22.04.

### Contribute [![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/salesagility/SuiteCRM/issues)

There are lots of ways to [contribute](https://docs.suitecrm.com/community/) to SuiteCRM

* [Submit bug](https://docs.suitecrm.com/community/raising-issues/) reports and help us [verify fixes](https://docs.suitecrm.com/community/contributing-code/test-pull-requests/) as they are pushed up
* Review and collaborate [source code](https://github.com/salesagility/SuiteCRM/pulls) changes
* Join and engage with other SuiteCRM users and developers on the [forums](https://suitecrm.com/suitecrm/forum)
* [Contribute bug fixes](https://docs.suitecrm.com/community/contributing-code/bugs/)
* Help [translate](https://docs.suitecrm.com/community/contributing-to-docs/contributing-to-translation/) language packs
* [Write and improve](https://docs.suitecrm.com/community/contributing-to-docs/) SuiteCRM documentation
* Signing CLA - Only needs to be done once for all PRs and contributions.


### Code Contributors

This project exists thanks to all the people who [contribute](https://github.com/salesagility/SuiteCRM/graphs/contributors) and more.
<a href="https://github.com/salesagility/SuiteCRM/graphs/contributors"><img src="https://opencollective.com/SuiteCRM/contributors.svg?avatarHeight=36&width=890&button=false" /></a>

You wanna buy the **core team** a coffee :coffee: or beer :beer:?
Then consider a small [donation](https://opencollective.com/SuiteCRM/contribute) to help fuel our activities :heart:

### Security ###

We take security seriously here at SuiteCRM so if you have discovered a security risk report it by
emailing [security@suitecrm.com](mailto:security@suitecrm.com). This will be delivered to the product team who handle security issues.
Please don't disclose security bugs publicly until they have been handled by the security team.

Your email will be acknowledged within 24 hours during the business week (Mon - Fri), and you’ll receive a more
detailed response to your email within 72 hours during the business week (Mon - Fri) indicating the next steps in
handling your report.

### Roadmap ### 

View the [Roadmap](https://suitecrm.com/roadmap/) and [LTS](https://suitecrm.com/lts/) for details on our planned features and future direction.

### Support ###

SuiteCRM is an open-source project. If you require help with support then please use our [support forum](https://suitecrm.com/suitecrm/forum/). By using the forums the knowledge is shared with everyone in the community. Our developer and community team members answer questions on the forum daily but it also allows the other members of the community to contribute. If you would like customisations to specifically fit your SuiteCRM needs then please visit the [website](https://suitecrm.com/).

### License [![AGPLv3](https://img.shields.io/github/license/suitecrm/suitecrm.svg)](./LICENSE.txt)

SuiteCRM is published under the AGPLv3 license.




