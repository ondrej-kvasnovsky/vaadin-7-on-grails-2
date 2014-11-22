# Spring Security

Spring Security in pure Grails application, where no Vaadin is involved, is provided by [spring-security-core](http://grails.org/plugin/spring-security-core) plugin, because if we work with pure Grails, the controllers are the end points and we can define security at that level, by adding Spring security annotations. This is not case for Vaadin application.

In Vaadin applications we use Navigator instead of Grails controllers or we render different UI components based on user's roles. In this chapter we will explore way to do using [Spring security framework](http://docs.spring.io/spring-security/site/docs/3.2.5.RELEASE/reference/htmlsingle/#core-components
).
