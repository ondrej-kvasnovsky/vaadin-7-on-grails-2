# Spring Security Basics

We will create basic classes that are required to work with Spring in Grails application.

The first step before you continue with this tutorial, is to enable [Open Session In View](#Open Session In View) to allow lazy loading in GORM. If you do not want OSIV in your project,  set lazy to false on `roles` collection in `User` class.

### Step 1

Create `Role` domain class that will represent a role assigned to a user. For example, a user can have multiple roles, like admin, client and so on.

``` java
grails create-domain-class app.security.Role
```

``` java
package app.security

class Role {

    String name

    static constraints = {
    }
}

```

### Step 2

Create `User` domain class that will represent your user.

``` java
grails create-domain-class app.security.User
```

Then implement `UserDetails` interface from Spring Security and provide all the required fields.

``` java
package app.security

import org.springframework.security.core.GrantedAuthority
import org.springframework.security.core.authority.SimpleGrantedAuthority
import org.springframework.security.core.userdetails.UserDetails

class User implements UserDetails {

    String username
    String password
    boolean accountNonExpired
    boolean accountNonLocked
    boolean credentialsNonExpired
    boolean enabled

    static hasMany = [roles: Role]

    static constraints = {
    }

    @Override
    Collection<? extends GrantedAuthority> getAuthorities() {
        println roles
        return roles.collect { Role authority ->
            new SimpleGrantedAuthority(authority.name)
        }
    }

    @Override
    boolean isAccountNonExpired() {
        return accountNonExpired
    }

    @Override
    boolean isAccountNonLocked() {
        return accountNonLocked
    }

    @Override
    boolean isCredentialsNonExpired() {
        return credentialsNonExpired
    }

    @Override
    boolean isEnabled() {
        return enabled
    }
}
```

### Step 3

Because we need a user with some roles during development, create a new user with few roles in `BootStrap`.

``` java
import app.security.Role
import app.security.User

class BootStrap {

    def init = { servletContext ->
        Role adminRole = new Role(name: 'ADMIN')
        adminRole.save(failOnError: true)
        Role userRole= new Role(name: 'USER')
        userRole.save(failOnError: true)

        User user = new User()
        user.username = "john"
        user.password = "john"
        user.enabled = true
        user.accountNonExpired = true
        user.accountNonLocked= true
        user.credentialsNonExpired = true
        user.roles = []
        user.roles << userRole
        user.roles << adminRole
        user.save(failOnError: true)
    }
    def destroy = {
    }
}
```

### Step 4

Now create `UserService` that will search for user by name and password in the database.

``` java
grails create-service app.security.UserService
```

We will use this to login the users.

``` java
package app.security

import grails.transaction.Transactional

@Transactional
class UserService {

    User findByUsernameAndPassword(String username, String password) {
        User user = User.findByUsernameAndPassword(username, password)
        return user
    }
}
```


### Step 5

Create your implementation of `AuthenticationManager` interface that will authentificate a user.

``` java
package app.security

import org.springframework.beans.factory.annotation.Autowired
import org.springframework.security.authentication.AuthenticationManager
import org.springframework.security.authentication.BadCredentialsException
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken
import org.springframework.security.core.Authentication
import org.springframework.security.core.AuthenticationException
import org.springframework.security.core.GrantedAuthority

class AuthManager implements AuthenticationManager {

    @Autowired
    private UserService userService

    public Authentication authenticate(Authentication auth) throws AuthenticationException {
        String username = (String) auth.principal
        String password = (String) auth.credentials

        User user = userService.findByUsernameAndPassword(username, password)
        if (user != null) {
            Collection<? extends GrantedAuthority> authorities = user.authorities
            return new UsernamePasswordAuthenticationToken(username, password, authorities)
        }

        throw new BadCredentialsException("Bad Credentials")
    }
}
```

Then define a new bean, because we need to have it present in the context and we want to autowire `UserService`.

``` java
import app.security.AuthManager

beans = {
    authenticationManager(AuthManager)
}
```

### Step 6

This can be done in many ways, but let's create a helper class `Auth` that will encapsulate authentification.

``` java
package app.security

import com.vaadin.grails.Grails
import org.springframework.security.authentication.AuthenticationManager
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken
import org.springframework.security.core.Authentication
import org.springframework.security.core.context.SecurityContextHolder

class Auth {

    static Authentication login(String username, String password) {
        UsernamePasswordAuthenticationToken request = new UsernamePasswordAuthenticationToken(username, password)

        AuthenticationManager authenticationManager = Grails.get(AuthenticationManager)
        Authentication result = authenticationManager.authenticate(request)

        SecurityContextHolder.context.authentication = result

        return result
    }
}
```
