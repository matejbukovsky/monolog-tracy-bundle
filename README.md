# [Tracy](https://tracy.nette.org) BlueScreen handler for [Symfony](https://symfony.com/) ([Monolog](https://github.com/Seldaek/monolog))

[![Build Status](https://img.shields.io/travis/nella/monolog-tracy-bundle/master.svg?style=flat-square)](https://travis-ci.org/nella/monolog-tracy-bundle)
[![Windows Build Status](https://img.shields.io/appveyor/ci/Vrtak-CZ/monolog-tracy-bundle/master.svg?style=flat-square)](https://ci.appveyor.com/project/Vrtak-CZ/monolog-tracy-bundle)
[![Code Coverage](https://img.shields.io/coveralls/nella/monolog-tracy-bundle.svg?style=flat-square)](https://coveralls.io/r/nella/monolog-tracy-bundle)
[![SensioLabsInsight Status](https://img.shields.io/sensiolabs/i/76c87979-7eda-4f6b-94a5-07bd54259d5f.svg?style=flat-square)](https://insight.sensiolabs.com/projects/76c87979-7eda-4f6b-94a5-07bd54259d5f)
[![Latest Stable Version](https://img.shields.io/packagist/v/nella/monolog-tracy-bundle.svg?style=flat-square)](https://packagist.org/packages/nella/monolog-tracy-bundle)
[![Composer Downloads](https://img.shields.io/packagist/dt/nella/monolog-tracy-bundle.svg?style=flat-square)](https://packagist.org/packages/nella/monolog-tracy-bundle)
[![Dependency Status](https://img.shields.io/versioneye/d/user/projects/569191a8daa0bf00330000db.svg?style=flat-square)](https://www.versioneye.com/user/projects/569191a8daa0bf00330000db)
![License MIT, GPL-2, GPL-3](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)

Sponzored by [Shipito LLC](https://www.shipito.com).

Bundle providing mainly integration of [Tracy](https://tracy.nette.org/) into [Symfony](https://symfony.com).

Are you looking for _Monolog_ integration only? There is [Monolog-Tracy](https://github.com/nella/monolog-tracy).

## Tracy - Blue Screen Handler

Long story short, Tracy helps you debug your applications when an error occurs providing you lots of information about what just happened. Check out
[live example](http://nette.github.io/tracy/tracy-exception.html) and [Tracy documentation](https://tracy.nette.org/)
to see the full power of this tool.

To replace default Symfony Bluescreen you can use [Tracy Bluescreen Bundle](https://github.com/VasekPurchart/Tracy-Blue-Screen-Bundle)
fully compatible with this library.

## Installation

Using  [Composer](http://getcomposer.org/):

```sh
$ composer require nella/monolog-tracy-bundle
```

### Register Bundle
```php
// AppKernel.php

public function registerBundles()
{
    $bundles = array(
        // ...
        new Nella\MonologTracyBundle\MonologTracyBundle(),
    );
}
```

### Register a new Monolog handler
```yml
monolog:
    handlers:
        blueScreen:
            type: tracyBlueScreen
```

## Configuration
Any error/exception making it to the top is automatically saved in `%kernel.logs_dir%/tracy`. You can easily change the log directory,
see full configuration options below:

```yml
# config.yml
monolog_tracy:
	log_directory: %kernel.logs_dir%/tracy # default
	handler_level: DEBUG # or 100 default - you can use int or constant name
	handler_bubble: true # default
	info_items:
		- Symfony 3.0.1 # default if HttpKernel is present
		- Doctrine ORM 2.5.2 # default if Doctrine ORM is present
		- Twig 1.23.1 # default if Twig is present
	panels: # no default panels
		- test.nella.monolog_tracy_bundle.panel.test_panel # callable ([class, method], [@service, method], @service, class::service)
	collapse_paths: # no default collapse paths
		- %kernel.root_dir%/vendor # TIP
```

This works out of the box and also in production mode!

## Tips

### Log notices/warnings in production

Use Symfony parameter `debug.error_handler.throw_at`: (see http://php.net/manual/en/function.error-reporting.php for possible values)
```yml
parameters:
    debug.error_handler.throw_at: -1
```

### Using Tracy\Debugger::dump

To prevent forgotten dumps to appear on production you can simply change the mode like this:
```php
// AppKernel.php

use Tracy\Debugger;

public function __construct($environment, $debug)
{
    Debugger::$productionMode = $environment === 'prod';
    parent::__construct($environment, $debug);
}
```

