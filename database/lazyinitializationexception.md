# LazyInitializationException

> Example code is available on
[github.com/vaadin-on-grails/gorm-LazyInitializationException](https://github.com/vaadin-on-grails/gorm-LazyInitializationException).

When we create a domain model that will cause lazy loading of other domain object, we might run into the troubles with `LazyInitializationException`.

### The Issue

In order to fully understand, lets show the code simulates  `LazyInitializationException`.

Create `Account` class that will just contain `name` property.

``` java
class Account {

    String name

    static constraints = {
    }
}
```

Now create class `User` that will belong to `Account` and in `toString()` method, just try to get value from `account` field. When that code is executed, Hibernate will try to load an account from database and will throw `LazyInitializationException`.

``` java
class User {

    String username
    String password

    static belongsTo = [account: Account]

    static constraints = {
    }

    public String getLabel() {
        return "$username - $account.name"
    }
}
```

In order to simulate the issue, here is the Vaadin code that will create a `ComboBox` with an instance of `User` class.

``` java
VerticalLayout layout = new VerticalLayout()

ComboBox userAccount = new ComboBox()

User user = User.findAll()[0]
BeanItem item = new BeanItem(user, 'label')

userAccount.addItem(item)

layout.addComponent(userAccount)
setContent(layout)
```

### How to prevent LazyInitializationException

The solution to avoid `LazyInitializationException`, is to disable lazy loading for class that should be also fetched at once with the other domain object.

Say to Hibernate to load account together with the user. Add the following code into `User` class.

``` java
static mapping = {
    account lazy: false
}
```

When you try to execute the application, you the account will be already loaded in `getLabel()` method and it will not fail.

This also makes sense from performance point of view, because we will avoid multiple database calls and the user and account will be loaded together, in single database call.

Another approach is to have open session for each request. Read the next two articles about Open Session In View to find out how to implement it in your application.

