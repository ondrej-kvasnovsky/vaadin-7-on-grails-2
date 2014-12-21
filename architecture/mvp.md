# Model View Presenter

> Example code is available on
[github.com/vaadin-on-grails/model-view-presenter](https://github.com/vaadin-on-grails/model-view-presenter).

This article shows an example of [Model View Presenter](http://en.wikipedia.org/wiki/Model–view–presenter) implementation. The basic idea is to show a way to split work with domain and service layer and UI view layer, not mixing all together.

> Martin Fowler wrote great article worth reading about [UI patterns](http://martinfowler.com/eaaDev/uiArchs.html).

What are the roles of MVP.

* *Model* represents database entities and provide way to manipulate with them.
* *View* displays UI components and has no context of how the database is used to fetch data.
* *Presenter* connect Model and View together, while only the presenter has the right to touch Model.

This example is intentionally simplified to easier understing of MVP implementation. We will create application that displays user name fetched from database.

### Step 1

Create domain model class for user entity. Run `grails create-domain-class app.model.User` command to create `User` class.

``` java
package app.model

class User {

    String firstname
    String surname

    String email

    static constraints = {
    }
}
```

### Step 2

Now create service, by running `grails create-service app.model.UserService` command, that will be used to manipulate with `User` domain object.

``` java
package app.model

import grails.transaction.Transactional

@Transactional
class UserService {

    User findByEmail(String email) {
        return User.findByEmail(email)
    }
}

```

### Step 3

Persist one user in `BootStrap.groovy` to have a database record in database. We will use it later.

``` java
import app.model.User

class BootStrap {

    def init = { servletContext ->

        User john = new User()
        john.firstname = 'John'
        john.surname = 'Lustig'
        john.email = 'john@app.com'
        john.save()

    }
    def destroy = {
    }
}
```


### Step 4

Now create a view to display details about user. We will never do any database operations in `UserView` class but instead we provide methods to populate values in UI.

``` java
package app.view

import app.model.User
import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout

class UserView extends VerticalLayout implements View {

    UserView() {
        setMargin(true)
    }

    @Override
    void enter(ViewChangeListener.ViewChangeEvent event) {
    }

    void showUserDetail(User user) {
        addComponent(new Label(user.firstname))
        addComponent(new Label(user.surname))
    }
}
```

> We could create an interface for `UserView` that will define all mandatory methods for that view. That would easier mocking of `UserView` in test and could give a way to make multiple implementations of user view.

### Step 5

Now we create presenter for `UserView`. It will have reference to `UserView` and `UserService`.

The presenter will be responsible to add a view into navigator and to fetch values using `UserService`.

``` java
package app.presenter

import app.model.User
import app.model.UserService
import app.view.UserView
import com.vaadin.grails.Grails
import com.vaadin.ui.UI

class UserPresenter {

    public static final String USER = 'user'

    UserView view

    UserPresenter() {
    }

    void initialize() {
        view = new UserView()
        UI.current.navigator.addView(USER, view)
    }

    void navigateTo() {
        UI.current.navigator.navigateTo(USER)

        UserService model = Grails.get(UserService)
        User user = model.findByEmail('john@app.com')
        view.showUserDetail(user)
    }
}
```

### Step 6

Now we use `UserPresenter` in our UI class to display user details.

``` java
package app

import app.presenter.UserPresenter
import com.vaadin.navigator.Navigator
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.UI

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        Navigator navigator = new Navigator(this, this)
        setNavigator(navigator)

        new UserPresenter().initialize().navigate()
    }
}
```

Now we can run the application and user will appear in the browser.

![MVP running app](http://vaadinongrails.com/book/3_1_mvp.png)


