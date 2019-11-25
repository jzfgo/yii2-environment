Yii2-environment
===============

[![Latest Stable Version](http://img.shields.io/packagist/v/marcovtwout/yii2-environment.svg?style=flat)](https://packagist.org/packages/marcovtwout/yii2-environment)
[![Total Downloads](http://img.shields.io/packagist/dt/marcovtwout/yii2-environment.svg?style=flat)](https://packagist.org/packages/marcovtwout/yii2-environment)
[![Scrutinizer Quality Score](http://img.shields.io/scrutinizer/g/marcovtwout/yii2-environment.svg?style=flat)](https://scrutinizer-ci.com/g/marcovtwout/yii2-environment/)

Environment-based configuration class (for example in development, testing, staging and production).

Simple class used to set configuration and debugging depending on environment.
Using this you can predefine configurations for use in different environments,
like _development, testing, staging and production_.

The main config (`main.php`) is extended to include the Yii 2 debug flag.
There are `mode_<environment>.php` files for overriding and extending main.php for specific environments.
Additionally, you can overrride the resulting config by using a `local.php` config, to make
changes that will only apply to your specific installation.

This class was designed to have minimal impact on the default Yii generated files.
Minimal changes to the index/bootstrap and existing config files are needed.

The Environment is determined with PHP's getenv(), which searches $_SERVER and $_ENV.
There are multiple ways to set the environment depending on your preference.
Setting the environment variable is trivial on both Windows and Linux, instructions included.
You can optionally override the environment on a per-project basis.

If you want to customize this class or its config and modes, extend it! (see [ExampleEnvironment.php](ExampleEnvironment.php))

## Requirements

Yii 2.*

## Installation

### Installation via Composer

1. Add the dependency to your project
    
    ```
    composer require marcovtwout/yii2-environment
    ```
2. Modify your `index.php` (and other bootstrap files)
3. Modify your `main.php` config file and add mode specific config files
4. Set your local environment (see next section)

### Setting environment

Here are some examples for setting your environment to `DEVELOPMENT`.

#### Windows

1. Go to: Control Panel > System > Advanced > Environment Variables
2. Add new SYSTEM variable: name = `YII_ENVIRONMENT`, value = `DEVELOPMENT`
 * Details: http://support.microsoft.com/kb/310519/en-us

#### Linux/Mac

1. Open profile files:
 * Global bash shell: `/etc/profile`
 * Apache (as service): `/etc/apache2/envvars`
2. Add the following line: `export YII_ENVIRONMENT="DEVELOPMENT"`
 * Details: http://www.cyberciti.biz/faq/linux-unix-set-java_home-path-variable/

#### Apache only (cannot be used for console applications)

1. Check if mod_env is enabled
2. Open your `httpd.conf` or create a `.htaccess` file
3. Add the following line: `SetEnv YII_ENVIRONMENT DEVELOPMENT`
 * Details: http://httpd.apache.org/docs/1.3/mod/mod_env.html#setenv

#### Project only

1. Create a file `mode.php` in the config directory of your application.
2. Set the content of the file to: `DEVELOPMENT`

## Usage

### Update bootstrap files

See [example-index/index.php](example-index/index.php)

#### Override environment

You can override the automatic determination of the current environment if needed:

* Pass the environment to the constructor: `new Environment('TEST')`
* For console applications, you can also do: `YII_ENVIRONMENT=STAGING ./yii`

### Structure of config directory

Your `protected/config/` directory will look like this:

```
config/main.php                     (Global configuration)
config/mode_development.php         (Environment-specific configurations)
config/mode_test.php
config/mode_staging.php
config/mode_production.php
config/local.php                    (Optional, local override for mode-specific config. Don't put in your SVN!)
```

### Modify your config/main.php

See [example-config/main.php](example-config/main.php)

Optional: in configConsole you can copy settings from configWeb by using value key `inherit` (see examples folder).

### Create mode-specific config files

Create `config/mode_<mode>.php` files for the different modes. These will override or merge attributes that exist in the main config.

- See [example-config/mode_development.php](example-config/mode_development.php)
- See [example-config/mode_test.php](example-config/mode_test.php)
- See [example-config/mode_staging.php](example-config/mode_staging.php)
- See [example-config/mode_production.php](example-config/mode_production.php)

Optional: also create a `config/local.php` file for local overrides.

## Credits and history

Based on the Yii 1 extension: https://github.com/marcovtwout/yii-environment
