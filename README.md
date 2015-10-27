# Testsuite
Selenium testsuite for the HAL management console. It uses [Drone](https://docs.jboss.org/author/display/ARQ/Drone) 
and [Graphene](https://docs.jboss.org/author/display/ARQGRA2/Home) Arquillian extensions.

## Prerequisites

* JDK 8 or higher
* Maven 3.0.4 or higher
* Firefox browser (tested on 31.2.0 esr version, will probably not run with far older or younger versions)
* Wildfly (10 or higher) or EAP (7 or higher)

You can download it here:
<http://wildfly.org/downloads/> or <http://www.jboss.org/products/eap/download/>

Currently it's necessary to unsecure the management http-interface to be able to run testsuite.
E.g. for standalone Wildfly 10 using CLI:
```
/core-service=management/management-interface=http-interface:undefine-attribute(name=security-realm)
:reload
```

## How to run tests

`mvn test -P{module},{server.mode} -Djboss.dist=${path_to_server_home} -Darq.extension.webdriver.firefox_binary=${path_to_firefox_binary}`

### Required profile (-P) parameters

Can be one of those:
* `-Pbasic,standalone` ... run basic tests against standalone mode
* `-Pbasic,domain` ... run basic tests against domain mode
* `-Prbac,standalone` ... run RBAC related tests against standalone mode
* `-Prbac,domain` ... run RBAC related tests against domain mode

### Required jboss.dist parameter

Path to server home folder. Server is expected to be already manually started. 
In case of standalone mode standalone-full-ha configuration is expected. 
E.g. `-Djboss.dist=/home/user/workspace/wildfly/build/target/wildfly-9.0.0.Alpha2-SNAPSHOT/`

### Optional arq.extension.webdriver.firefox_binary parameter

Path to Firefox binary file. If not provided system default firefox will be used.
E.g. `-Darq.extension.webdriver.firefox_binary=/home/user/apps/firefox-31.2.0esr/firefox`

### Optional take.screenshot.after.each.test parameter

If screenshot should be made after each test. Default is `false`.
E.g. `-Dtake.screenshot.after.each.test=true`

## Modules

### common

Should contain all common abstraction like UI navigation, test categories, DMR abstraction etc.
It's intentioned to be able to used as a dependency of external testsuites as well.

### basic

Should contain majority of tests which don't require special configuration.

### rbac

Should contain RBAC related tests.

## Tips

* If you want tests to be run on background use vncserver. E.g. 

`vncserver :10 -geometry 1920x1080`

`export DISPLAY=:10`


## Known issues

* No sreenshots on test failure neither test error currently (It seems Arquillian unlike Surefire thinks they passed).

## Problems?

Ping us on IRC freenode.net#wildfly-management
[Issue tracking](https://issues.jboss.org/browse/HAL/)

Have fun.
