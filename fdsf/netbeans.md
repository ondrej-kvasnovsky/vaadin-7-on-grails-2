# NetBeans

> Working example project, which results from this tutorial, can be downloaded from [github.com/vaadin-on-grails/quickstart-app](https://github.com/vaadin-on-grails/quickstart-app).

You need to install full version of [NetBeans](https://netbeans.org/downloads/index.html) (where Groovy is included).

Then you need to install [Grails plugin](https://netbeans.org/kb/docs/web/grails-quickstart.html) for NetBeans. Search for `grails` and mark `Groovy and Grails` plugin as selected.

Click on `Install` button and finish the instalation which will require restart Netbeans IDE.

![Install Grails plugin](http://vaadinongrails.com/book/1_5_plugin.png)

### Step 1

Open `New Project` window to create Grails application. Select `Grails Application` and click on `Next`.

![Install Grails plugin](http://vaadinongrails.com/book/1_5_create_project.png)

### Step 2

Type name of the project and finish the wizard.

![Install Grails plugin](http://vaadinongrails.com/book/1_5_project_name.png)

Also configure Grails version that will be used for your project.

![Install Grails plugin](http://vaadinongrails.com/book/1_5_grails.png)

### Step 3

Open `BuildConfig.groovy` and add dependency to vaadin plugin `compile ":vaadin:7.3.3.2"`.

> The latest version of the plugin is always available on http://grails.org/plugin/vaadin

![Add dependency](http://vaadinongrails.com/book/1_5_buildconfig.png)

Open commands window in Netbeans.

![Open commands window](http://vaadinongrails.com/book/1_5_open-grails.png)

Run `grails compile` command. It will download Vaadin plugin together with all mandatory dependencies.

![Add dependency](http://vaadinongrails.com/book/1_5_run-compile.png)

Then click on `Refresh Commands` button and run `grails vaadin-quickstart`  that will generate sample code to easier project configuration.

![Add dependency](http://vaadinongrails.com/book/1_5_run-quickstart.png)

The command `vaadin-quickstart` will generate `MyUI.groovy` and remove URL mapping inside `UrlMapping.groovy`.


##### Optional

We want to prevent Grails to take over URL mapping. Open `UrlMappings.groovy` and make sure the URL mapping is empty.

```java
class UrlMappings {
    static mappings = {
    }
}
```

### Step 5

We are ready to start up the application. Run `grails run-app` command.

![run-app target](http://vaadinongrails.com/book/1_5_run_app.png)

Vaadin application running on http://localhost:8080/my-app will become accessible after a while.

![Running Vaadin application](http://vaadinongrails.com/book/1_1_run_app.png)
