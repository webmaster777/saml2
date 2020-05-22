SimpleSAMLphp SAML2 library
===========================
![CI](https://github.com/simplesamlphp/saml2/workflows/CI/badge.svg?branch=master)
[![Build Status](https://travis-ci.org/simplesamlphp/saml2.png?branch=feature/fix-build)](https://travis-ci.org/simplesamlphp/saml2)
[![Coverage Status](https://codecov.io/gh/simplesamlphp/saml2/branch/master/graph/badge.svg)](https://codecov.io/gh/simplesamlphp/saml2)

A PHP library for SAML2 related functionality. Extracted from [SimpleSAMLphp](https://www.simplesamlphp.org),
used by [OpenConext](https://www.openconext.org).
This library started as a collaboration between [UNINETT](https://www.uninett.no) and [SURFnet](https://www.surfnet.nl) but everyone is invited to contribute.


Before you use it
-----------------
**DO NOT USE THIS LIBRARY UNLESS YOU ARE INTIMATELY FAMILIAR WITH THE SAML2 SPECIFICATION.**

If you are not familiar with the SAML2 specification and are simply looking to connect your application using SAML2,
you should probably use [SimpleSAMLphp](https://www.simplesamlphp.org).

Note that the **HTTP Artifact Binding and SOAP client do not work** outside of SimpleSAMLphp.

Which version to pick?
----------------------
The latest released version (`4.x` range) is the _preffered version_.
The `3.x branch` is our LTS branch and will be supported as long as [SimpleSAMLphp](https://www.simplesamlphp.org) hasn't migrated to the `4.x` branch.

All other branches (`2.x` and earlier) are no longer supported and will not receive any maintenance or
(security) fixes. Do not use these versions.

We conform to [Semantic Versioning](https://semver.org/).
Be sure to check the [UPGRADING.md](UPGRADING.md) file if you are upgrading from an older version. Here
you will find instructions on how to deal with BC breaking changes between versions.

Usage
-----

* Install with [Composer](https://getcomposer.org/doc/00-intro.md), run the following command in your project:

```bash
composer require simplesamlphp/saml2:^3.0
```

* Provide the required external dependencies by extending and implementing the ```SAML2\Compat\AbstractContainer```
  then injecting it in the ContainerSingleton (see example below).

* **Make sure you've read the security section below**.

* Use at will.
Example:
```php
    // Use Composers autoloading
    require 'vendor/autoload.php';

    // Implement the Container interface (out of scope for example)
    require 'container.php';
    SAML2\Compat\ContainerSingleton::setContainer($container);

    // Set up an AuthnRequest
    $request = new SAML2\AuthnRequest();
    $request->setId($container->generateId());
    $issuer = new SAML2\XML\saml\Issuer();
    $issuer->setValue('https://sp.example.edu');
    $request->setIssuer($issuer);
    $request->setDestination('https://idp.example.edu');

    // Send it off using the HTTP-Redirect binding
    $binding = new SAML2\HTTPRedirect();
    $binding->send($request);
```

Security
--------
* Should you need to create a DOMDocument instance, use the `SAML2\DOMDocumentFactory` to create DOMDocuments from
  either a string (`SAML2\DOMDocumentFactory::fromString($theXmlAsString)`), a file (`SAML2\DOMDocumentFactory::fromFile($pathToTheFile)`)
  or just a new instance (`SAML2\DOMDocumentFactory::create()`). This in order to protect yourself against the
  [XXE Processing Vulnerability](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing), as well as
  [XML Entity Expansion](https://phpsecurity.readthedocs.org/en/latest/Injection-Attacks.html#defenses-against-xml-entity-expansion) attacks

License
-------
This library is licensed under the LGPL license version 2.1.
For more details see [LICENSE](https://raw.github.com/simplesamlphp/saml2/master/LICENSE).
