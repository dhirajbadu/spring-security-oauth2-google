# Spring Security OAuth2 Google Plugin
### Clone
```
git clone https://github.com/dhirajbadu/spring-security-oauth2-google.git
```

### Dependencies
````
 compile 'org.grails.plugins:spring-security-core:4.0.3'
 compile 'org.grails.plugins:spring-security-oauth2:1.3.0.BUILD-SNAPSHOT'
````
### Installation
Add the following in build.gralde of you mail app
```
grails {
    plugins {
        compile project(':spring-security-oauth2-google')
    }
}
```
Add the following in setting.gradle
````
include('spring-security-oauth2-google')
````

### Configs
Add the following configs in you application.yml
````
plugin:
        springsecurity:
            oauth2:
                active: true
                registration:
                    askToLinkOrCreateAccountUri: '/oauth2/ask'
                    roleNames: ["ROLE_USER","ROLE_ADMIN"]
                    supplyUserDetails:
                        userDetailFrom: 'GrailsUser' # values 'GrailsUser', 'userDetailsService

                providers:
                    google:
                        api_key: 'your api key'
                        api_secret: 'your api secret'
                        successUri: '/oauth2/google/success'
                        failureUri: '/oauth2/google/failure'
                        callback: '/oauth2/google/callback'
                        scopes: 'https://www.googleapis.com/auth/userinfo.email'
````
