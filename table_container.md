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

Leave only `web.xml` and remove folders `artifacts`, `scaffolding` and `testing`.

![Generated templates](http://vaadinongrails.com/book/2_1_6_OSIV.png)

### Step 2
