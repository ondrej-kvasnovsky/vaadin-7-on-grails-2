# Create Sql

> Example code is available on
[github.com/vaadin-on-grails/groovy-sql](https://github.com/vaadin-on-grails/groovy-sql).

We can use [Groovy Sql](http://groovy.codehaus.org/api/groovy/sql/Sql.html) class to access a database. The proper way to create new instance of `Sql` class is to define new bean in `resources.groovy`.

``` java
import groovy.sql.Sql

beans = {
    sql(Sql, ref('dataSource')) { bean ->
        bean.destroyMethod = 'close'
    }
}
```

If, from any reason, you would need to create `Sql` manually, you can the alternative approach to create `Sql` each time before SQL execution.

``` java
Sql sql = new Sql(dataSource: Grails.applicationContext.getBean('dataSource'))
// execute your queries
sql.close()
```
