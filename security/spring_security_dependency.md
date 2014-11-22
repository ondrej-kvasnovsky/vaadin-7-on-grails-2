# Spring Security Dependency

Add dependency to [spring-security-core](http://mvnrepository.com/artifact/org.springframework.security/spring-security-core) into `BuildConfig.groovy`. We need to exclude several libraries to get it working.


``` java
dependencies {
    String springSecurityVersion = '3.2.5.RELEASE'

    compile "org.springframework.security:spring-security-core:$springSecurityVersion", {
        excludes 'aopalliance', 'aspectjrt', 'cglib-nodep', 'commons-collections', 'commons-logging', 'ehcache', 'fest-assert', 'hsqldb', 'jcl-over-slf4j', 'jsr250-api', 'junit', 'logback-classic', 'mockito-core', 'powermock-api-mockito', 'powermock-api-support', 'powermock-core', 'powermock-module-junit4', 'powermock-module-junit4-common', 'powermock-reflect', 'spring-aop', 'spring-beans', 'spring-context', 'spring-core', 'spring-expression', 'spring-jdbc', 'spring-test', 'spring-tx'
    }
}
```

> Get the version from [Grails Spring security plugin](https://github.com/grails-plugins/grails-spring-security-core/blob/master/grails-app/conf/BuildConfig.groovy).
