# Transactions

> Example code is available on
[github.com/vaadin-on-grails/gorm-transactions](https://github.com/vaadin-on-grails/gorm-transactions).

Grails [service layer](http://grails.org/doc/latest/guide/single.html#services) is the proper place to put all the application logic. Services are transactional and also good place to make your applications code reusable.

In this article will show how to store user with a new account in a transaction. So, when account is saved but saving user fails the transaction is rolled back. That results in scenario that nothing is storred, user nor account.

### Step 1

Create domain class that represents an account: `grails create-domain-class com.myapp.Account`.


``` java
class Account {

    static constraints = {
    }
}
```

### Step 2

Create domain class that represents an user that will have a reference to an accont: `grails create-domain-class com.myapp.User`.

``` java
class User {

    String username
    String password

    static belongsTo = [account: Account]

    static constraints = {
    }
}
```

### Step 3

In this step we create `AccountService` that will be transactional, meaning that all methods in the service will be executed in a transaction. When an exception that extends RuntimeException is thrown, the transaction will be rolled back automatically.

> It is also possible to define transactional behavior on [method level](http://grails.org/doc/latest/guide/services.html#declarativeTransactions).

Run `grails create-service com.myapp.AccountService` to create the service.

``` java
import grails.transaction.Transactional

@Transactional
class AccountService {

    void createUserWithAccount(String username, String password) {
        Account account = new Account()
        account.save(failOnError: true)

        User user = new User()
        user.username = username
        user.password = password
        user.account = account
        user.save(failOnError: true)

        throw new RuntimeException("unexpected exception to simulate failure")
    }
}
```

> Instead of defining `failOnError: true` everywhere, it is possible to enable [failOnError globaly](http://grails.org/doc/latest/ref/Domain%20Classes/save.html) `grails.gorm.failOnError = true` in `Config.groovy`.

### Step 4

Create a view that will execute `createUserWithAccount` with dummy values. After the method is executed, try to fetch values from database and print it into the console to see nothing is stored in database because we let the transaction fail.

``` java
package app

import com.myapp.Account
import com.myapp.AccountService
import com.myapp.User
import com.vaadin.grails.Grails
import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Button
import com.vaadin.ui.VerticalLayout

class AccountView extends VerticalLayout implements View {

    static final String VIEW_NAME = "account"

    @Override
    void enter(ViewChangeListener.ViewChangeEvent viewChangeEvent) {

        Button btn = new Button("Create account")
        btn.addClickListener(new Button.ClickListener() {
            @Override
            void buttonClick(Button.ClickEvent clickEvent) {
                AccountService accountService = Grails.get(AccountService)

                try {
                    accountService.createUserWithAccount('test', 'password')
                } catch (Exception e) {
                    println "failed: ${e.message}"
                }

                println Account.findAll()
                println User.findAll()
            }
        })

        addComponent(btn)
    }
}
```

### Step 3

In order to test the behavior, we can use the account view in MyUi class.

```
package app

import com.vaadin.navigator.Navigator
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.UI

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest vaadinRequest) {

        Navigator navigator = new Navigator(this, this)

        navigator.addView(AccountView.VIEW_NAME, AccountView)

        navigator.navigateTo(AccountView.VIEW_NAME)
    }
}
```

When we run the application and a runtime exception is thrown in the service method, we should see no database items fetched from the database. Have a look the console output to see what has happend.

```
failed: database connection failed
[]
[]
```

After we remove throwing a runtime exception exception:

``` java
    void createUserWithAccount(String username, String password) {
        Account account = new Account()
        account.save(failOnError: true)

        User user = new User()
        user.username = username
        user.password = password
        user.account = account
        user.save(failOnError: true)
    }
```

We will see that rows has been inserted into the database. Have a look the console output to see what has happend.

```
[com.myapp.Account : 1]
[com.myapp.User : 1]
```
