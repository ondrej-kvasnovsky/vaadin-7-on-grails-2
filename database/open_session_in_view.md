# Open Session In View I.

> Example code is available on
[github.com/vaadin-on-grails/gorm-open-session-in-view-i](https://github.com/vaadin-on-grails/gorm-open-session-in-view-i).

As described in LazyInitializationException article, we will run into troubles with `LazyInitializationException` when domain object tries to load other object lazily. If the disabling of lazy loading is not what we are looking for and we still want to do lazy fetching, we have to use OSIV (Open Session In View).

Stephan Grundner has implemented adding OSIV filter into the plugin. Thanks to that we can configure `openSessionInViewFilter` property in `VaadinConfig.grovy`.

``` java
openSessionInViewFilter = 'org.springframework.orm.hibernate4.support.OpenSessionInViewFilter'
```

Or add older Hibernate filter in case you are using Hibernate 3.

``` java
openSessionInViewFilter = 'org.springframework.orm.hibernate3.support.OpenSessionInViewFilter'
```



