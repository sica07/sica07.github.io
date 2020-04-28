# PhpConference 2019

[<TIL](Programming.md)
- [Workshop](#Workshop)
- [Practical Security for Web Applications](#Practical Security for Web Applications)
- [Architecture Hands-On](#Architecture Hands-On)
- [Static code analysis in PHP](#Static code analysis in PHP)
- [Migrating from bare metal machines to aws ecs](#Migrating from bare metal machines to aws ecs)
- [Tools of the Trade](#Tools of the Trade)
- [An Introduction to Architecture Katas](#An Introduction to Architecture Katas)
- [PHP to Hack at Slack](#PHP to Hack at Slack)
- [PHPUnit Best Practices](#PHPUnit Best Practices)
- [ReactPHP](#ReactPHP)
- [Let's go frameworkless to focus on the domain again](#Let's go frameworkless to focus on the domain again)

## Workshop
- focus on the business problem, the domain
- composer should be used only you need dependencies for runtime, not for develop time
- in phpunit `--testdox` uses the meaningful name of the test as a indicator
- self-defence technique: `final` for classes, `private` for properties
- when dealing with returning integers, we should return a value object that tells
  us what that integer represents. For example, when returning the price of a
  good, we should have a Pound class that contains the price. The reason for that
  is the fact that we want to be sure that we interact with the correct data type.
  When summing two integers, the program doesn't know if the two integers refer to
  the same datatype so the only way to be sure that this two integers represent
  the same datatype (price + price != price + distance) is using value objects.
- collaborators vs. dependecies: collaborators are, generally, object values that
  do not need mocks in unit tests because they just incapsulate values and don't
  have too much logic that needs to be tested.

## Practical Security for Web Applications
Tools for scanning PHP project:
* sonarqube.org
* progpilot
* phpcs-security-audit
* OWASP WAP_Web_Application_Protection
* roave/security-advisoriesw methods
* owtf.org (penetration testing)
* MetaSploit

Injection:
* use prepared statements with named parameter binding
* in active record be aware of raw syntax
Use TLS for everything (free tls letsencrypt.org)
* jsencrypt -> encrypt the password on client with the public key, send it encrypted
with TLS (2nd layer), decrypted on server with the private key

Event log:
* separate db with separate credentials
* user id, IP address, timestamp, operation (beware of GDPR)

Do not expose data stores to internet
Only web server should be able to connect to DB

## Architecture Hands-On
- try the most not to assume anything
- test only the business logic

## Static code analysis in PHP
- logical errors (needs tests)
- compile time error (needs linter) **php-parallel-lint**
- runtime errors
- check `generic syntax`
- check `assert()` but enable it only in development
- doctrine/coding-standard for php-cs-fixer
- use `value objects` for important integers and `enums` for the state
- PHPStorm + PhpInspections
- psalm, phan, phpstan (has plugins for frameworks)
- start at level 0 (phpstan)
- talks.benjamin-cremer.de/ipc_sca

## Migrating from bare metal machines to aws ecs
- ecs-deploy
- aws-env

## Tools of the Trade
- php-cs-fixer: can reorder the methods in a class
    - can replace instances where spaceship operator can be used
    - `.php_cs.dist` to configurate
- psalm: check _type coverage_
- phpcpd
- dePHPend
- infection.phar
- object-graph
- tideways.com (profiling) php-xhprof-extension, toolkit, phpunit-tideway-listener

## An Introduction to Architecture Katas
- http://code-quality.de
- software architecture are those decisions that are hard to change
- weeks of coding can save you hours of planing

## PHP to Hack at Slack
- focus on the return type first because it impacts lot of code
- focus on argument types later because it impacts only the function scope

## PHPUnit Best Practices
- use a supported version of PHPUnit (PHPUnit 7 and 8)
- use version constraints in composer "^8.0" (not "*", ">=8", etc)
- do not install PHPUnit globally

## ReactPHP
- async I/O operations
- the *event loop* is the core component
- *stream* => an abstraction for something too huge to store in the memory (e.g. files)
- *sockets* => streams but over the network
- *promises* => placeholder for a single future result
- *event source* => Content-Type: text/event-stream
  the connection remains open. It's like websockets but with plain text

## Let's go frameworkless to focus on the domain again
- framework = dependency
- how to avoid dependencies: domain driven design
- adapt the framework to the domain not the other way around
- use the framework as it was just a library
- slides: bit.ly/php-fw-ddd
