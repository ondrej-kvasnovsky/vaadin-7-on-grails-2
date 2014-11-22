# Work With Domain Objects

In case an application would be just fetching data from a database, kind of reporting application, we could theoretically, use just domain object to fetch data from the database.

``` java
List<Item> items = Item.findAll()
```

But usually, we want to parametrize fetching data from database (apply limits, pass parameters for searching).

When there is a need to update in a transaction or do more complex logic around. We would tend to use [withTransaction](http://grails.org/doc/latest/ref/Domain%20Classes/withTransaction.html) to ensure both items are stored in signle transaction.

``` java
Item.withTransaction { status ->
    Item item1 = new Item(label: "first item")
    Item item2 = new Item(label: "second item")

    item1.save()
    item2.save()
}
```


Also, the application code shoudn't repeat the same code in mupltiple places. If we fill up the Vaadin code with direct calling of domain classes the code might become messy and difficult to read.

Therefore we should put all work with domain object into services. [Service layer](http://grails.org/doc/latest/guide/services.html) article explains how to work with domain objects in Grails.


