# Create domain model

> Example code is available on
[github.com/vaadin-on-grails/gorm-domain](https://github.com/vaadin-on-grails/gorm-domain).

Grails documentation has nice description how to work with [domain](http://grails.org/doc/latest/guide/single.html#domainClasses) classes. In case you are new to Grails domain classes, you should definitelly read it.

Anyway, let's show an example how to create a domain object and use it in Vaadin application.

### Step 1

Run Grails `create-domain` command that will create the new domain class `Item`.

``` java
grails create-domain-class com.app.Item
```

The generated and empty domain class `Item.groovy` is in `grails-app/domain/com/app` folder.

### Step 2

Before we start testing the domain class, we need to add there at least one field, for example, a string `label`.

``` java
package com.app

class Item {

    String label

    static constraints = {
    }
}
```

> Domain object must be stored in `grails-app/domain`, contain at static field `constraints` to be considered as a valid domain object. Only then a database table for a domain object will be created.

### Step 3

Now, we need to create few records of `Item` in the database during the application start-up.

Open `BootStrap.groovy` and save few `Item` instances there.

``` java
import com.app.Item

class BootStrap {

    def init = { servletContext ->
        new Item(label: 'First').save()
        new Item(label: 'Second').save()
        new Item(label: 'Third').save()
        new Item(label: 'Fourth').save()
    }

    def destroy = {
    }
}

```

### Step 4

Use domain class to fetch all records from the database and display them in the browser.

``` java
import com.app.Item
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest vaadinRequest) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        List<Item> items = Item.findAll()
        for (Item item : items) {
            String label = item.label
            layout.addComponent(new Label(label))
        }

        setContent(layout)
    }
}
```

Run the application and database records will be displayed in UI.

![Records from database displayed in browser](http://vaadinongrails.com/book/2_1_1_domains_in_browser.png)
