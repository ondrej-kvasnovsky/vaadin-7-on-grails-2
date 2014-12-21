# Application With Navigator

> Example code is available on
[github.com/vaadin-on-grails/spring-autowiring-navigator](https://github.com/vaadin-on-grails/spring-autowiring-navigator).

In this tutorial we will show how to initialize `Navigator` and how to implement `View`.

The first two steps are the same as for the previous 'Create Simple Application'.

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

In this step we will do the following things:
* Create new UI that extends `DefaulUI`
* Use `@VaadinUI` annotation to do URL mapping for `MyUI`
* Call `super.init(r)` method that will initialize the navigator

After the point mentioned above are done, we can use `Views` class to open a view. Continue to the next step to learn how views are registered using `@VaadinUI` annotation.

``` java
package app

import com.vaadin.grails.navigator.Views
import com.vaadin.grails.ui.DefaultUI
import com.vaadin.grails.ui.VaadinUI
import com.vaadin.server.VaadinRequest

@VaadinUI(path = '/')
class MyUI extends DefaultUI {

    @Override
    protected void init(VaadinRequest r) {
        super.init(r)

        Views.enter(ItemView)
    }
}
```

### Step 4

Before we use `Views.enter(View)` method we need to tell the pluging where are the views and what is the URL path to them.

Add `@VaadinUI` annotation to every view that should be accesible by `Views.enter` method.

``` java
package app

import com.vaadin.grails.navigator.VaadinView
import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout
import org.springframework.beans.factory.annotation.Autowired

@VaadinView(path = "item")
class ItemView extends VerticalLayout implements View {

    @Autowired
    ItemService itemService

    @Override
    void enter(ViewChangeListener.ViewChangeEvent e) {
        setMargin(true)

        String label = itemService.serviceMethod()
        addComponent(new Label(label))
    }
}
```

### Step 5

Run the application `grails run-app` and open http://localhost:8080/spring-autowiring-navigator in a browser.

![Running application with Navigator](http://vaadinongrails.com/book/4_2_navigator.png)
