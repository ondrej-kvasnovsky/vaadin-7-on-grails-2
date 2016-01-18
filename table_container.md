# Table Container

> Example code is available on
[github.com/vaadin-on-grails/gorm-vaadin-table](https://github.com/vaadin-on-grails/gorm-vaadin-table).

In the previous article we have described how to enable OSIV in VaadinConfig.groovy. This tutorial will show way how to do it in the old way, without configuration.

The other way to use OSIV in Grails with Vaadin is to manually add an extra filter `OpenSessionInViewFilter` into `web.xml` file.

### Step 1

Create a domain object `Item` with two String fields `name` and `other`.

``` java
package com.vaadinongrails

class Item {

    String name
    String other

    static constraints = {
    }
}
```

### Step 2

Create few records of Item in database in `BootStrap.groovy` file.

``` java
class BootStrap {

    def init = { servletContext ->
        new Item(name: "Hi 1", other: "There").save(failOnError: true)
        new Item(name: "Hi 2", other: "There").save(failOnError: true)
        new Item(name: "Hi 3", other: "There").save(failOnError: true)
        new Item(name: "Hi 4", other: "There").save(failOnError: true)
    }
    def destroy = {
    }
}
```

### Step 3

Create grid.


### Step 4

Runn application and see the table with GORM objects from database.



gorm-vaadin-table.png