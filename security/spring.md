# Secured Navigator

> Example code is available on
[github.com/vaadin-on-grails/spring-security-basics](https://github.com/vaadin-on-grails/spring-security-basics).

In this article we will show way how to create Vaadin application with secured views, where a view will be viewable by certain role.

### Step 1

In `ViewSecurity` class we will store the mapping of a view to user roles. The class will be also responsible to verify whether a view is accesible by a user.

``` java
package app.security

import com.vaadin.navigator.View
import org.springframework.security.authentication.InternalAuthenticationServiceException
import org.springframework.security.core.Authentication
import org.springframework.security.core.GrantedAuthority
import org.springframework.security.core.context.SecurityContextHolder

class ViewSecurity {

    private static Map<Class<? extends View>, List<String>> views = [:]

    static add(Class<? extends View> view, List<String> roles) {
        views.put(view, roles)
    }

    static boolean isViewAccessible(View view) {

        List<String> roles = views.get(view.class)
        if (!roles) {
            // if roles is null, the access is public (not secured)
            return true
        }

        Authentication authentication = SecurityContextHolder.context.authentication
        if (!authentication) {
            throw new InternalAuthenticationServiceException('No authentication found in the context.')
        }

        List<GrantedAuthority> authorities = authentication.authorities

        for (String role : roles) {
            boolean isRoleAssigned = role in authorities*.authority
            if (isRoleAssigned) {
                return true
            }
        }

        return false
    }
}
```

### Step 2

We need to hook at event when a view changes, so we can verify whether a user is able to access a view.

``` java
package app.security

import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener

class SecuredViewChangeListener implements ViewChangeListener {

    @Override
    boolean beforeViewChange(ViewChangeListener.ViewChangeEvent event) {
        View view = event.newView
        boolean isViewAccessible = ViewSecurity.isViewAccessible(view)

        return isViewAccessible
    }

    @Override
    void afterViewChange(ViewChangeListener.ViewChangeEvent event) {
    }
}
```

### Step 3

Now we will create two views where one will be accesible only by "ADMIN" role.

``` java
package app.view.secured

import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout

class UserDataView extends VerticalLayout implements View {

    static final String VIEW_NAME = "secured"

    @Override
    void enter(ViewChangeListener.ViewChangeEvent event) {
        setMargin(true)

        addComponent(new Label("Secured content, only after login!"))
    }
}
```

And the second view, `LoginView`, that will public to everyone.

``` java
package app.view.login

import app.security.Auth
import app.view.secured.UserDataView
import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Button
import com.vaadin.ui.TextField
import com.vaadin.ui.VerticalLayout
import org.springframework.security.authentication.BadCredentialsException
import org.springframework.security.core.Authentication
import org.springframework.security.core.GrantedAuthority

import static com.vaadin.ui.UI.getCurrent

class LoginView extends VerticalLayout implements View {

    static final String VIEW_NAME = "login"

    @Override
    void enter(ViewChangeListener.ViewChangeEvent viewChangeEvent) {
        TextField txnUsername = new TextField("Username")
        TextField txnPassword = new TextField("Password")
        Button btnLogin = new Button("Login")

        addComponent(txnUsername)
        addComponent(txnPassword)
        addComponent(btnLogin)

        btnLogin.addClickListener(new Button.ClickListener() {

            @Override
            void buttonClick(Button.ClickEvent clickEvent) {
                String username = txnUsername.value
                String password = txnPassword.value

                try {
                    Authentication auth = Auth.login(username, password)

                    List<GrantedAuthority> authorities = auth.authorities
                    if ('ADMIN' in authorities*.authority) {
                        current.navigator.navigateTo(UserDataView.VIEW_NAME)
                    } else {
                        current.navigator.navigateTo(LoginView.VIEW_NAME)
                    }

                } catch (BadCredentialsException e) {
                    // TODO: handle error flow
                    current.navigator.navigateTo(LoginView.VIEW_NAME)
                }
            }
        })
    }
}
```

### Step 4

Now we will create `UI` class that will initialize `Navigator` and `ViewSecurity`.

``` java
package app

import app.security.SecuredViewChangeListener
import app.security.ViewSecurity
import app.view.login.LoginView
import app.view.secured.UserDataView
import com.vaadin.annotations.PreserveOnRefresh
import com.vaadin.navigator.Navigator
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

@PreserveOnRefresh
class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Navigator navigator = new Navigator(this, this)

        SecuredViewChangeListener securedViewListener = new SecuredViewChangeListener()
        navigator.addViewChangeListener(securedViewListener)

        navigator.addView(LoginView.VIEW_NAME, LoginView)
        navigator.addView(UserDataView.VIEW_NAME, UserDataView)

        ViewSecurity.add(LoginView, null)
        ViewSecurity.add(UserDataView, ['ADMIN'])

        navigator.navigateTo(LoginView.VIEW_NAME)

        setContent(layout)
    }
}
```

### Step 5

Run application `grails run-app` and fill in username and password.

![Login screen](http://vaadinongrails.com/book/6_3_spring-security-login.png)

Click on `Login` button and you will be authentificated and redirected to secured view.

![Login screen](http://vaadinongrails.com/book/6_3_spring-security-secured.png)

