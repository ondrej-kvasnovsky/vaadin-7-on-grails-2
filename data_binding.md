# Spring Autowiring

> Managing Vaadin classes in Spring context has been added recently. Please report all the issues you find.

Recently we have added support for [autowiring](http://docs.spring.io/spring-framework/docs/2.5.x/api/org/springframework/beans/factory/annotation/Autowired.html).
That makes `Grails.get()` method, which returns beans from application context, useless.

This chapter shows how to configure and create sample application using new annotations `@VaadinUI` and `@VaadinView`.

