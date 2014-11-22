# Basics

When we add dependency to [Vaadin plugin](http://grails.org/plugin/vaadin) into `BuildConfig.groovy` and run `grails compile` the plugin generates the following files inside our Grails application.

## MyUI.groovy

Sample UI class `src/groovy/app/MyUI.groovy` which contains code that just shows a label in the web browser.

We can immediatelly start coding Vaadin application using that file, or maybe rather do a refactoring. For example, rename the file to `AppUI.groovy` and move it to different package (folder) `src/groovy/com/company/app`. Then you need to change `mapping` property inside `VaadinConfig.groovy`.

## VaadinConfig.groovy

Vaadin configuration file `grails-app/conf/VaadinConfig.groovy` contains all the configuration that is required by the plugin.

We will explore what all can be configured in `VaadinConfig.groovy`.

### mapping

We can provide multiple Vaadin UI classes that extends `com.vaadin.ui.UI` and they will be mapped to specified URL patterns. There must be at least one UI class.

Let's look at this example where we map three URL paters, each to separate UI class.

``` java
    mapping = [
        "/*": "app.MyUI",
        "/admin/*": "app.AdminUI",
        "/client/*": "app.ClientUI"
    ]
```

The application UIs will then become available at:
* http://localhost:8080/grails-vaadin7-demo
* http://localhost:8080/grails-vaadin7-demo/client
* http://localhost:8080/grails-vaadin7-demo/admin

> Be aware, creating multiple UIs is not the way how to navigate in the Vaadin applicatin. Use [views navigator](https://vaadin.com/book/-/page/application.architecture.html#application.architecture.navigation) for this.


### mappingExtras

We need to define extra mapping in case we need to 'reserve' a URL that should not be mapped to, for example, `/*` by Vaadin.

For example, we need to enable [console](http://grails.org/plugin/console) plugin. First we add plugin dependency `compile ":console:1.4.4"` into `BuildConfig.groovy`.

Then we provide mapping for `/console/*` pattern.

    mappingExtras = [
        '/console/*'
    ]

To get console plugin working, you need to apply [this hack](https://github.com/ondrej-kvasnovsky/grails-console/commit/f2e302d879e8c8ba1643f78b4cf3e78f21984e6e).

### productionMode

When [production mode](https://vaadin.com/book/vaadin6/-/page/advanced.debug-production-modes.html) is set to false, it enables debug mode. So, we can easily inspect application in browser by adding `?debug` to the end of URL.

By default, the productionMode is set to false also inside Grails.

    productionMode = false

In order to enable `productionMode` in production, make sure the following code is present.

    environments {
        production {
            vaadin {
                productionMode = true
            }
        }
    }

### asyncSupported

Set this property to true to enable asynchronous communication, so you can use [Vaadin push](https://vaadin.com/book/vaadin7/-/page/advanced.push.html).

    asyncSupported = true

### themes

You can provide name of the [themes](https://vaadin.com/book/vaadin7/-/page/themes.creating.html), which is a directory name in web-app/VAADIN/themes folder.

    themes = ['sample']

### sassCompile

You can specify exact version of Vaadin for SASS compilation.

    sassCompile = '7.2.5'

### widgetset

In order to use your own widget set, for example, if you need to use add-ons.

    widgetset = 'com.mycompany.widgetset'

If `widgetset` is not set, the default widget set from Vaadin is used.

### servletClass

There is a default servlet, [`com.vaadin.grails.server.DefaultServlet`](https://github.com/ondrej-kvasnovsky/grails-vaadin-plugin/blob/master/grails-vaadin7-plugin/src/groovy/com/vaadin/grails/server/DefaultServlet.groovy),  provided by Vaadin plugin that makes actually possible to run Vaadin inside Grails.

If you need to create your own servlet, you can do it by extending [`com.vaadin.grails.server.DefaultServlet`](https://github.com/ondrej-kvasnovsky/grails-vaadin-plugin/blob/master/grails-vaadin7-plugin/src/groovy/com/vaadin/grails/server/DefaultServlet.groovy). Then set `servletClass` to your custom servlet.

    servletClass = "com.app.MyGrailsAwareApplicationServlet"

### packages

We can define a package name where Spring will search for components.

    packages = ['com.mycompany.vaadin']

This is optional, all packages will get scanned by default.

### uiProvider

We can create own implementation of [`com.vaadin.server.UIProvider`](https://vaadin.com/wiki/-/wiki/Main/Creating+an+application+with+different+features+for+different+clients).

    uiProvider = "com.mycompany.MyGrailsAwareUIProvider"

This is optional, default is [`com.vaadin.grails.server.DefaultUIProvider`](https://github.com/ondrej-kvasnovsky/grails-vaadin-plugin/blob/master/grails-vaadin7-plugin/src/groovy/com/vaadin/grails/server/DefaultUIProvider.groovy).

### openSessionInViewFilter

In order to activate Open Session in View for Hibernate 3.

    openSessionInViewFilter = 'org.springframework.orm.hibernate3.support.OpenSessionInViewFilter'

Or this for Hibernate 4.

    openSessionInViewFilter = 'org.springframework.orm.hibernate4.support.OpenSessionInViewFilter'
