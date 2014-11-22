# Clean Up With Alternatives

With alternative database frameworks, like Groovy Sql, MyBatis, JdbcTemplate, we do not need some GORM libraries, like hibernate, let's remove them and maybe exclude some others.

Before you start excluding and removing dependencies, let's run `grails dependency-report compile` command to see what libraries are loaded for compile scope.

### Step 1

Remove `runtime ":hibernate4:4.x"` or `runtime ":hibernate:3.x"` from `BuildConfig.groovy`.

### Step 2

Add exclude for `grails-plugin-databinding` in `BuildConfig.groovy`.

``` java
grails.project.dependency.resolution = {
    inherits("global") {
        'grails-plugin-databinding'
    }
```
