# Open Session In View II.

> Example code is available on
[github.com/vaadin-on-grails/gorm-open-session-in-view-ii](https://github.com/vaadin-on-grails/gorm-open-session-in-view-ii).

In the previous article we have described how to enable OSIV in VaadinConfig.groovy. This tutorial will show way how to do it in the old way, without configuration.

The other way to use OSIV in Grails with Vaadin is to manually add an extra filter `OpenSessionInViewFilter` into `web.xml` file.

### Step 1

Generate project templates in your project. `install-templates` command will generate many files inside `src/templates` folder.

``` java
grails install-templates
```

Leave only `web.xml` and remove folders `artifacts`, `scaffolding` and `testing`.

![Generated templates](http://vaadinongrails.com/book/2_1_6_OSIV.png)

### Step 2

Add the following filter definition, that will keep session opened during each reqiest, into generated `web.xml`.

``` xml
<!-- OSIV Filter -->
<filter>
    <filter-name>openSessionInView</filter-name>
    <filter-class>org.springframework.orm.hibernate4.support.OpenSessionInViewFilter</filter-class>
</filter>

<filter-mapping>
    <filter-name>openSessionInView</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

> Change package name of `OpenSessionInViewFilter` for Hibernate 3 `org.springframework.orm.hibernate3.support.OpenSessionInViewFilter`

We are done. Now we can run the application and Hibernate session will be always opened, during each Vaadin request.
