# Basics

This article shows how to work with MyBatis in Groovy application. We will fetch data from database and show them on UI.

### Step 1

Create `Item` class that will represent a database entity. This class will be used by MyBatis as a data transfare object. MyBatis will automatically fetch values from database in to an instance of `Item` class.

``` java
package com.app.mappers

class Item {

    Long id
    String label
}
```

### Step 2

Create mapper interface for `Item` class. Then this mapper will be referenced from XML mapping file.

``` java
package com.app.mappers

interface ItemMapper {

    Item findById(Long id)
}
```

### Step 3

Create `src/groovy/com/app/mappers/ItemMapper.xml` [XML mapper](http://mybatis.github.io/mybatis-3/sqlmap-xml.html) file and define SQL query that will be executed when `findById` method is called.

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.app.mappers.ItemMapper">
    <select id="findById" resultType="com.app.mappers.Item">
        select * from Item where id = #{id}
    </select>
</mapper>
```

### Step 4

To try out whether MyBatis is working, create `Item` database table and insert there a new record. So, we can fetch it later in Vaadin UI.

``` java
import javax.sql.DataSource
import java.sql.Statement

class BootStrap {

    DataSource dataSource

    def init = { servletContext ->

        Statement statement = dataSource.getConnection().createStatement()

        statement.execute("CREATE TABLE Item (id INTEGER NOT NULL, label VARCHAR(255))")
        statement.execute("INSERT INTO Item (id, label) VALUES (1, 'Sample')")

        statement.close()
    }

    def destroy = {
    }
}
```

### Step 5

Here is an example of how to get the mapper and fetch values from database. Get the bean from the context `Grails.get(ItemMapper)` and call the method `findById(1)`.

`ItemMapper` implementation is provided by [mybatis-spring](http://mybatis.github.io/spring/).

``` java
package app

import com.app.mappers.Item
import com.app.mappers.ItemMapper
import com.vaadin.grails.Grails
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest request) {

        VerticalLayout layout = new VerticalLayout()

        ItemMapper itemMapper = Grails.get(ItemMapper)
        Item item = itemMapper.findById(1)

        Label label = new Label(item.label)
        layout.addComponent(label)

        setContent(layout)
    }
}
```

When `grails run-app` is executed the application is started, a value `Sample` is fetched from database and displayed in the web browser.

![Running application with MyBatis](http://vaadinongrails.com/book/2_2_2_run-app.png)
