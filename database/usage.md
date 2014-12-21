# Usage

> Example code is available on
[github.com/vaadin-on-grails/jdbc-template](https://github.com/vaadin-on-grails/jdbc-template).

This article shows a few ways how to use JDBC template by Spring.

### JdbcTemplate with parameters

The default way how to use JdbcTemplate with parameters.

``` java
JdbcTemplate jdbcTemplate = Grails.get(JdbcTemplate)

Object[] args = new Object[1]
args[0] = 1

int[] argsTypes = new int[1]
argsTypes[0] = Types.INTEGER

Map userFromDb = jdbcTemplate.queryForMap("SELECT * FROM User WHERE id=?", args, argsTypes)

String firstName = userFromDb.get('first_name')
layout.addComponent(new Label(firstName))
```

### NamedParameterJdbcTemplate with parameters

Named parameters are more readable form of passing parameters into a query.

``` java
NamedParameterJdbcTemplate namedJdbcTemplate = Grails.get(NamedParameterJdbcTemplate)

Map params = [id: 1]

List<Map> usersX = namedJdbcTemplate.queryForList("SELECT * FROM User WHERE id=:id", params)

usersX.each {
    String firstName = it.get('first_name')
    layout.addComponent(new Label(firstName))
}
```

> If you use [jOOQ](http://www.jooq.org) for SQL string construction, there is a comfortable way to get [named parameters](http://www.jooq.org/doc/3.4/manual/sql-building/bind-values/named-parameters/) as a map.

``` java
SelectSelectStep select = ...
Map<String, Object> getParams() {
    Map params = select.params.collectEntries { String key, Param value ->
        [(key): value.value]
    }
    return params
}
```

### NamedParameterJdbcTemplate with parameters and RowMapper

Create a row mapper that will map result of a query to a domain object.

``` java
class UserRowMapper implements RowMapper {

    @Override
    Object mapRow(ResultSet rs, int rowNum) throws SQLException {
        return new User(firstName: rs.getString('first_name'))
    }
}
```

Now we can give `UserRowMapper` to named JDBC template and it will map the result of the query into a new instance of `User` class.

``` java
NamedParameterJdbcTemplate namedJdbcTemplate = Grails.get(NamedParameterJdbcTemplate)

Map params = [id: 1]
UserRowMapper mapper = new UserRowMapper()

User usersY = namedJdbcTemplate.queryForObject("SELECT * FROM User WHERE id=:id", params, mapper)

layout.addComponent(new Label(usersY.firstName))
```

> Use [BeanPropertyRowMapper](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/jdbc/core/BeanPropertyRowMapper.html) in case your domain model matches name of columns in a database table. Then you do not have to create mappers and data from your queries will automatically transfered to instance of a domain class.
