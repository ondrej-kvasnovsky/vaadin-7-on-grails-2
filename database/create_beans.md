# Create Beans

> Example code is available on
[github.com/vaadin-on-grails/jdbc-template](https://github.com/vaadin-on-grails/jdbc-template).

In this article, we will show how to define beans in order to start working with [spring-jdbc](http://docs.spring.io/autorepo/docs/spring/4.0.6.RELEASE/spring-framework-reference/html/jdbc.html).

### Step 1

Add dependency to [spring-jdbc](http://mvnrepository.com/artifact/org.springframework/spring-jdbc) into `BuildConfig.groovy`.

``` java
dependencies {
    compile 'org.springframework:spring-jdbc:4.0.6.RELEASE'
}
```

### Step 2

We can use `JdbcTemplate` to directly execute SQL queries, but if there will be inputs from users, we have to use `?` as a parameter placeholder or rather use `NamedParameterJdbcTemplate` to prevent [SQL injection](http://en.wikipedia.org/wiki/SQL_injection).

``` java
import org.springframework.jdbc.core.JdbcTemplate
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate

beans = {

    jdbcTemplate(JdbcTemplate, ref('dataSource'))

    namedJdbcTemplate(NamedParameterJdbcTemplate, ref('dataSource'))
}
```
