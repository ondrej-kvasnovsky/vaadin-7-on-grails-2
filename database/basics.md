# Execute SQLs

> Example code is available on
[github.com/vaadin-on-grails/groovy-sql](https://github.com/vaadin-on-grails/groovy-sql).

In order to execute a query with [Groovy Sql](http://groovy.codehaus.org/api/groovy/sql/Sql.html) just get `dataSource` bean from the application context, which we have created it in Create Sql article.

``` java
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.grails.Grails
import groovy.sql.GroovyResultSet
import groovy.sql.Sql

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest vaadinRequest) {
        VerticalLayout layout = new VerticalLayout()

        Sql sql = Grails.get(Sql)
        sql.eachRow("SELECT * FROM User") { GroovyResultSet result ->
            String firstName = result.getString('username')
            layout.addComponent(new Label(firstName))
        }

        setContent(layout)

        // Sql sql = new Sql(dataSource: Grails.applicationContext.getBean('dataSource'))
        // execute your queries
        // sql.close()
    }
}
```

> Constructing SQL strings in Java or Groovy code is not easy and it is, for some of us, annoying. I recommend to use [jOOQ](jooq.org) library to construct SQL strings.
