# SQLContainer

In this article we will show a way how to use [SQLContainer](https://vaadin.com/book/vaadin7/-/page/sqlcontainer.html) in Grails.

Reason why we need to take extra steps to get SQLContainer working, is that the default data source provided by Grails will get closed and returned to data source pool after a request is done. But Vaadin wants to access the data source even after it is closed by Grails.

That is the reason why `This connection has been closed` exception will be throw when Vaadin tries to access a data source from `J2EEConnectionPool`.

``` java
DataSource ds = Grails.get(DataSource)
JDBCConnectionPool pool = new J2EEConnectionPool(ds)
```

#### Step 1

Add new bean into `grails-app/conf/spring/resources.groovy` that will override the default `dataSource` bean provided by Grails.

``` java
import com.vaadin.grails.datasource.GrailsDataSource

beans = {
    dataSource(GrailsDataSource)
}```

#### Step 2

Use it in your Vaadin code.

``` java
DataSource dataSource = Grails.applicationContext.getBean('dataSource')
JDBCConnectionPool pool = new J2EEConnectionPool(dataSource)

TableQuery query = new TableQuery("user", pool)
SQLContainer container = new SQLContainer(query)
Table table = new Table("Testing")
table.setContainerDataSource(container)
```
