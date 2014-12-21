# Configuration

> Example code is available on
[github.com/vaadin-on-grails/mybatis](https://github.com/vaadin-on-grails/mybatis).

Because Grails is based on Spring, we can use [mybatis-spring](http://mybatis.github.io/spring/) library to integrate [MyBatis](http://mybatis.github.io/mybatis-3/index.html) into Grails.

### Step 1

Add [mybatis](http://mvnrepository.com/artifact/org.mybatis/mybatis) and [mybatis-spring](http://mvnrepository.com/artifact/org.mybatis/mybatis-spring) dependencies into `BuildConfig.groovy`.

``` java
compile 'org.mybatis:mybatis:3.2.7'
compile 'org.mybatis:mybatis-spring:1.2.2'
```

### Step 2

Define new beans in `resources.groovy`.

*  `sqlSessionFactory` configures connection factory using XML files at the specified location
*  `mapperScannerConfigurer` will scan for mappers based on what is defined by `basePackage` property

``` java
import org.apache.commons.dbcp.BasicDataSource
import org.mybatis.spring.SqlSessionFactoryBean
import org.mybatis.spring.mapper.MapperScannerConfigurer

beans = {

    sqlSessionFactory(SqlSessionFactoryBean) {
        dataSource = ref("dataSource")
        mapperLocations = "classpath:com/app/mappers/**/*.xml"
    }

    mapperScannerConfigurer(MapperScannerConfigurer) {
        basePackage = "com.app.mappers"
    }
}

```


