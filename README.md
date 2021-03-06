
# Visual regression testing made easy

Automating the boring stuff. 

Managing a website can be difficult. You can end up with hundreds of pages, and making sure all of them are functional after an event, say a deployment, peaks of traffic, editorial changes, ads or javascripts behaving badly, ... Creating software and maintaining websites and apps is hard... regression testing should not.

# Requirements

- PHP 7.4
- Sqlite
- A (maybe big) list of urls to crawl. Robots.txt and Sitemaps detected automatically

Alternatively, using the Vagrant machine provided will take care of the sqlite and PHP 7.4 requirements. Keep reading.

# Install

1. Clone / fork this repository.

2. Run:
```composer install```

3. Make a copy of your config.php

```cp config.sample.php config.php```

# Usage

Create a .csv which contains a list of urls to iterate over (see example.csv).

If using Acquia Site Factory, a command is supplied to generate a list of sites from a sites.json file. You'll need to:

1. Download the sites.json in your Acquia Cloud subscription

```scp  [subscription][ENV].[ENV]@[subscription][ENV].ssh.enterprise-g1.acquia-sites.com:/mnt/files/[subscription][ENV]/files-private/sites.json ./sites-dev.json```

2. Vagrant up if you want to use the crawler inside the virtual machine (recommended).


3. Run the crawl against that json

```php bin/visual_regression_bot.php acquia:acsf-crawl-sites sites.json```


You can see all available commands by running:

```php bin/visual_regression_bot.php```

For help with a specific command use:

```php bin/visual_regression_bot.php help <command>```

Whilst debugging, increase verbosity by adding a number of `-v` flags.

`-v` : verbose
`-vv` : very verbose
`-vvv` : debug

# Configuration

There are some settings that you can configure, like the headers that you'll send to the site or the concurrency that you want to use.

Move your config.sample.php into config.php and adapt to your needs. For example:

```
<?php

return [
  'headers' => [
    'User-Agent'   => 'GlitcherBotScrapper/0.1',
    'Accept'       => 'application/json',
  ],
  'http_errors' => false,
  'connect_timeout' => 0, // wait forever
  'read_timeout' => 0,
  'timeout' => 0, // wait forever
  'concurrency' => 60,
];

```

Note: The higher the concurrency is configured, the more sites it will run on each step, but be careful, php is fast (contrary to popular believ) and can send high load to a site and put it in trouble. Big power means bigger responsibility.

# Docker Web App

A basic docker compose file is included that will run a web server for viewing results.

To run this:

1. Ensure that your dependencies have been installed using composer.
2. Run the container using 'docker-compose up -d'

The server runs on port 10002 on localhost, so navigate to 'http://localhost:10002' to get started.
