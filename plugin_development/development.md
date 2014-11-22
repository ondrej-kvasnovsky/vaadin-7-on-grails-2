# Development

Create a sample Grails application with Vaadin plugin that you will use for plugin development.

There are two ways to use the plugin in the sample application.

#### Way 1

Add direct dependency to BuildConfig.groovy. You can add it as the last row there.

``` java
grails.plugin.location.'vaadin' = "/Users/john/projects/grails-vaadin7-plugin"
```

Then all the changes do in Vaadin plugin will become immediatelly available in the sample application.

#### Way 2

The second way is to release plugin into local maven repository. Before you do so, you need to set plugin version to SNAPSHOT.

``` java
grails set-version 7.3.0-SNAPSHOT
```

Then you reference SNAPSHOT version in the sample application. You can do the changes in Vaadin plugin and release it as a snapshot to your local maven repository.

``` java
grails maven-install
```

> It is recommended to test SNAPSHOT version in the sample application everytime before you create a pull request.
