XeroPHP
-----------------------
A client implementation of the [Xero API](<http://developer.xero.com>), with a cleaner OAuth interface and ORM-like abstraction.

## Background

I hate reinventing the wheel, but this was written out of desperation. I wasn't comfortable putting the implementation that's recommended by Xero in to production, even after persisting with extending it.

This is loosely based on the functional flow of XeroAPI/XeroOAuth-PHP, but is split logically into more of an OO design.

## Main changes
* Variables are named clearly and only defined if actually used
* Methods are only defined in one place
* Project is split into useful components rather than one massive class
* Organised methods so it's more clear what's going on and how to debug
* More robust implementation of signing methods
* Removal of countless semantic issues

This library has been tested with Private, Public and Partner apps but is still a WIP, I'd love contributions/fixes from anyone that is keen to join the cause!

### Model Generation

Any files in the XeroPHP/Models directory are system generated.  Ideally, these shouldn't be modified directly, as it will be difficult to track/update.  Instead, if you notice something wrong with them, have a look at the ```generate/``` folder.  This contains the generation code, which actually just scrapes <http://developer.xero.com/documentation/> and parses out model/property/relation information.

## Requirements
* PHP 5.3+
* php\_curl extension - ensure a recent version (7.30+)
* php\_openssl extension

## Setup

Using composer:

```json
  "require": {
  	"calcinai/xero-php": "1.1.*"
  }
```

Otherwise just download the package and add it to your autoloader.  Namespaces are PSR-4 compliant.

## Usage

Create a XeroPHP instance (sample config included):

```php
$xero = new \XeroPHP\Application\PrivateApplication($config);
```

Load a collection of objects and loop through them
```php
$contacts = $xero->load('Accounting\\Contact')->execute();
foreach ($contacts as $contact) {
    print_r($contact);
}
```

Load something by its GUID
```php
$contact = $xero->loadByGUID('Accounting\\Contact', [GUID]);
```

Or create & populate it
```php
$contact = new \XeroPHP\Models\Accounting\Contact();
$contact->setName('Test Contact')
			->setFirstName('Test')
			->setLastName('Contact')
			->setEmailAddress('test@example.com');
```

Save it
```php
$xero->save($contact);
```

Refer to the [examples](examples) for more complex usage and nested/related objects.
