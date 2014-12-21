# Create Simple Application

> Example code is available on
[github.com/vaadin-on-grails/spring-autowiring-basics](https://github.com/vaadin-on-grails/spring-autowiring-basics).

In this tutorial, we will create Vaadin application where we can use `@Autowired` annotation.

### Step 1

We need to say the plugin that we want to do annotation based URL mapping. Comment out mapping from `VaadinConfig.groovy` to do that.

``` java
vaadin {
//    mapping = [
//            "/*": "app.MyUI"
//    ]
}
```

### Step 2

Now we will create a dummy service `ItemService` that we will autowire in Vaadin code.

``` java
package app

import grails.transaction.Transactional

@Transactional
class ItemService {

    String serviceMethod() {
        return 'Value from service'
    }
}
```

### Step 3

Use `@VaadinUI` annotation to map your UI class to URL.

``` java
package app

import com.vaadin.grails.ui.VaadinUI
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout
import org.springframework.beans.factory.annotation.Autowired

@VaadinUI(path = '/')
class MyUI extends UI {

    @Autowired
    ItemService itemService

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        String homeLabel = itemService.serviceMethod()

        Label label = new Label(homeLabel)
        layout.addComponent(label)

        setContent(layout)
    }
}
```

That is all, now you can run the application and `ItemService` will be autowired.

### Step 5

Run the application `grails run-app` and open http://localhost:8080/spring-autowiring-basics in a browser.

![Running application with Navigator](http://vaadinongrails.com/book/4_2_basics.png)

