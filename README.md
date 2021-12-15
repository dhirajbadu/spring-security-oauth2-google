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
Add the following in build.gradle of you mail app
```
grails {
    plugins {
        compile project(':spring-security-oauth2-google')
    }
}
```
Add the following in settings.gradle (if not then create the file )
````
include('spring-security-oauth2-google')
````
* Note: This plugin should be inside you main module repo
   #### PROJECT_DIR
    - settings.gradle
    - yourapp
        - build.gradle
    - spring-security-oauth2-google
        - build.gradle
* Reference: https://docs.grails.org/4.0.12/guide/plugins.html    
### Configs
Add the following configs in you application.yml
````
grails:
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
### Controller
````
    @Secured("permitAll")
    def index() {
        if(springSecurityService?.isLoggedIn()){
            redirect(action:'googleUser')
        }else{
            render(view: '/index')
        }
    }

    @Secured("ROLE_USER")
    def googleUser() {
        def principle = springSecurityService.principal
        render "hello ${principle.username} : you are google user"
    }
````
### Note
If the application is unable to build due to of an exception like "No GORM implementations configured. Ensure GORM has been initialized correctly" then remove the code below from  method doWithApplicationContext() of class spring-security-oauth2-google/src/main/groovy/spring/security/oauth2/google/SpringSecurityOauth2GoogleGrailsPlugin.groovy(inside plugin) and put that code to your applications Bootstrap.groovy
````
 def SpringSecurityOauth2BaseService oAuth2BaseService = grails.util.Holders.grailsApplication.mainContext.getBean('springSecurityOauth2BaseService') as grails.plugin.springsecurity.oauth2.SpringSecurityOauth2BaseService
        def GoogleOAuth2Service googleOAuth2Service = grails.util.Holders.grailsApplication.mainContext.getBean('googleOAuth2Service') as GoogleOAuth2Service
        try {
            oAuth2BaseService.registerProvider(googleOAuth2Service)
        } catch (OAuth2Exception exception) {
            log.error("There was an oAuth2Exception", exception)
            log.error("OAuth2 Google not loaded")
        }
````
### Reference
https://github.com/dhirajbadu/grails4_oauth2_social_login/

#### How to reach me?
* Email: dhirajbadu50@gmail.com
* LinkedIn: https://www.linkedin.com/in/dhiraj-badu-41648b134/

### Customization
If you need any contumization on the existing logic or you need other provider like okta, facebook, github, etc then please feel free to email me.
