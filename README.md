# spring-cloud-config-service-for-gits
A Spring cloud config service backed by multiple git repositories.

Simple unauthenticated service which provides configuration details for various applications. Each application may drive its  configuration with a personal git repository. pom.xml was built with [Spring Initializr](http://start.spring.io/) with Web and Config Server dependencies.

As usual, configuration values are stored in git repositories as .yml or .properties files.

Example application.yml:
```
spring:
  cloud:
    config:
      server:
        git:
          uri: file:/tmp/git/config-service/common
          clone-on-start: true
          repos:
            app1:
              pattern: app1-*
              uri: file:/tmp/git/config-service/app1
            app2:
              pattern: app2-*
              uri: file:/tmp/git/config-service/app2`
```

The first git.uri describes the default git repository. git.repos describe the other, application-specific git repositories.

Configuration files use the same naming convention as with a single git repository; all of app1's files must be named app1-something-profile.yml or app1-something-profile.properties. The app1-* pattern just tells the service to look for these files in the config-service/app1 repository.

http://config-service-uri/application/profile

The "application" name may contain dash characters, so feel free to build a configuration directory which resembles:
- app1-routes-production.yml
- app1-routes-development.yml
- app1-captions-production.yml
- app1-captions-development.yml
- app1-endpoints-production.yml
- app1-endpoints-development.yml

The third would be accessed at http://config-service-uri/app1-captions/production.
