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

Create instance of `Grid` in Vaadin code. We need to do two things to see GORM objects nicely displayed in the grid: 

1. Create new `BeanItemContainer` and add GORM objects into it
2. GORM object contains more fields then we have defined and we have to pick what columns will be visible. Then we have to remove the others which are not supposed to be visible.


``` java
package app

import com.vaadin.data.util.BeanItemContainer
import com.vaadin.grails.ui.VaadinUI
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Grid
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout
import com.vaadinongrails.Item

@VaadinUI(path = '/')
class MyUI extends UI {

    @Override
    protected void init(VaadinRequest vaadinRequest) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Grid grid = new Grid()
        grid.setSelectionMode(Grid.SelectionMode.SINGLE)

        BeanItemContainer<Item> container = new BeanItemContainer<>(Item.class)
        List<Item> all = Item.findAll()
        container.addAll(all)
        grid.setContainerDataSource(container)

        grid.setColumnOrder("id", "name", "other")
        grid.removeColumn("attached")
        grid.removeColumn("metaClass")
        grid.removeColumn("properties")
        grid.removeColumn("version")
        grid.removeColumn("dirty")
        grid.removeColumn("dirtyPropertyNames")
        grid.removeColumn("errors")

        layout.addComponent(grid)

        setContent(layout)
    }
}
```

### Step 4

Run application and see the table with GORM objects from database.


![MVP running app](http://vaadinongrails.com/book/gorm-vaadin-table.png)
