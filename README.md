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
In this config if you want to authenticate the user from the user table then you need to put
````
 supplyUserDetails:
                 userDetailFrom: 'userDetailsService'

````
else if you just want to populated the principal in srpingSecurityContext then put
````
 supplyUserDetails:
                 userDetailFrom: 'GrailsUser'

````
Where the authorities are taken from roleNames(must be in list).

If you dont want to populate the principal then remove the supplyUserDetails from config.
### In View(GSP)
````
<oauth2:connect provider="google" id="google-connect-link">Google</oauth2:connect>

Logged with google?
<oauth2:ifLoggedInWith provider="google">yes</oauth2:ifLoggedInWith>
<oauth2:ifNotLoggedInWith provider="google">no</oauth2:ifNotLoggedInWith>
````
#### How to reach me?
* Email: dhirajbadu50@gmail.com
* LinkedIn: https://www.linkedin.com/in/dhiraj-badu-41648b134/