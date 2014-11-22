# GORM

Grails Object Relational Mapping (GORM) provides easy way to define your database mode and it comes for free with Grails.

Usually the only place you need to configure, to get GORM working with your database, is `DataSource.groovy`. Grails will create bean `dataSource` that will be used for fetching data from database and that will reflect data source configuration you have set there.

> If you want to provide external configuration file, for example you deploy application to several production environments, you can define [external config](http://grails.org/doc/latest/guide/conf.html#configExternalized) and set the data source configuration there.
