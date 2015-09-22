# Vaadin on Grails

The book describes how to create project, setup configuration, what code is created automatically and how to clean up the code that will become useless when we use Vaadin for user interface.

We will go through two approaches how to work with database. The first one is GORM, which is the default option. And then few alternatives to GORM that are MyBatis, JdbcTemplate and Groovy Sql.

We will look data binding tricks, basic ideas for architecture, how to compose the components using Model View Presenter (MVP).

The book also contains tutorials how to compile SASS and widget set, after you add an add-on. How to use Spring security, localization and how to add a REST API from a Vaadin application and many other things.

If you are completly new to both technlogies, the book will show you first steps to start up and then you can continue with [Vaadin](https://vaadin.com/learn) and [Grails](http://grails.org/doc/latest/guide/single.html) documentation.

If you are both Vaadin and also Grails expert, you might look at the book as a reference book, set of tutorials and also a place with several solutions that you might have to solve in your applications.

## Behind The Scenes
This book could be written thanks to:
* Questions you have been asking during last few years about development of Vaadin using Groovy and Grails.
* Many years spend with development of applications using Grails and Vaadin.
* Chapter about Grails in [Vaadin 7 Cookbook](http://www.packtpub.com/creating-rich-internet-applications-in-vaadin-7/book) that showed there is more to discover.

## Vaadin

Vaadin provides both simple and complex web [UI components](http://demo.vaadin.com/sampler) that can be displayed in browser or in [mobile app](https://vaadin.com/directory#addon/vaadin-touchkit), while the client side, an applicaiton running inside a web browser, is still connected to the server and communication happens when required, after user's action or when server pushes data to the client from the server.

Developers who like object oriented programing and design patterns that result in predictable APIs, might feel excited about the way how the development of an user interface can be done with Vaadin. We can play with UI components and still keep thinking about it as object structures in Java.

## Grails

Grails 2.x is supported in the book. Support for Grails 3.x is comming soon.

Grails is framework used to build applications in really short time, because it provides:

* Dependency management
* Dependency injection with Spring application context
* URL mapping
* Filters
* Controllers
* Groovy Server Pages (.gsp) & tags
* AJAX support
* REST, SOAP
* GORM (Grails ORM)
* Transactional service layer
* Async behavior
* Internationalization i18n
* Environments
* Using Groovy and Java languages
* Groovy-powered Gant command line
* On the fly reloading

The most useful feature of Grails might be GORM (Grails Object Relation Mapping) that makes really easy to create a database layer. Then there are services, which support transactions, that are the place to put the application logic.

## Rich internet applications in Grails

In case you want to build a rich JavaScript client, you need to invest quite some time to build client side components. Which makes sense if we need to develop own components with special features (for example new generation of paginated table that looks like a chart). But sometimes, we need to build applications that should rather focus on solving customers issues and implementation of  business logic rather than spend time on implementation of JavaScript components. If that is the case, we could choose Vaadin to implement our Rich Internet Applications (RIA).

## Vaadin plugin

There is a Grails plugin called [vaadin](http://grails.org/plugin/vaadin) that integrates Vaadin into Grails realm, by providing dependencies to Vaadin libraries. Also, the plugin provides some support for development.
* Create start-up code with sample application
* Help to access beans and localization values
* SASS compilation
* Widgetset compilation

## Thanks

Thanks for buying the book and supporting the first official and complete documentation for Vaadin integration with Grails.

Also you will support development of the plugin and enhancement of this book. Which means, you can suggest what should be covered in this book.

Thanks for supporting [gitbook](https://gitbook.io), which is great platform for book publishing, because you give them 20% of the book price. Which is still much less than publishers charge.


