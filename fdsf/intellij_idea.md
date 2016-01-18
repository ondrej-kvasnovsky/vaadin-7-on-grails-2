# IntelliJ IDEA

> Working example project, which results from this tutorial, can be downloaded from [github.com/vaadin-on-grails/quickstart-app](https://github.com/vaadin-on-grails/quickstart-app).

Grails support in [IntelliJ IDEA](http://www.jetbrains.com/idea) is added through [Grails plugin](http://www.jetbrains.com/idea/webhelp/grails.html). Make sure it is installed in your IntelliJ IDEA before you start with the tutorial.

### Step 1

Go to http://grails.org/download and get the latest version of Grails. Unpack the Grails archive on your local computer and start up IntelliJ IDEA.

### Step 2

Open the `New Project` window and select `Grails` from the list and click on `Next` button.

##### Optional

If it is your first Grails project in IDEA, click on `Create...` button and then select the root of your unpacked Grails archive. It will add Grails into IDEA.

![Setup Grails](http://vaadinongrails.com/book/1_1_2_setup-idea.png)

### Step 3

Fill in the name of the project and choose the latest version of Grails.

![Project name](http://vaadinongrails.com/book/1_1_2_new-project.png)

### Step 4

Click on `Finish` and on the next dialog, choose `Run 'create app'`.

![Finish](http://vaadinongrails.com/img/create-app-idea.png)

> Warning: sometimes, IDEA is not able to create Grails project and you end up with weird errors. In that case, try to upgrade IDEA version or run `grails create-app my-app` in console. Then after that open the project in IDEA.

### Step 5

Open file `BuildConfig.groovy` and add Vaadin plugin `compile ":vaadin:7.6.1"`.

![Vaadin plugin](http://vaadinongrails.com/book/1_1_2_build-config-idea.png)

> The latest version of the plugin is always available on http://grails.org/plugin/vaadin

Now we need to tell IDEA to reload dependencies. Run `grails compile` or refresh the dependencies as shown on the following picture.

![Vaadin plugin](http://vaadinongrails.com/book/1_1_2_refresh-idea.png)

> `Synchronize Grails setting` should be performed always when you run into issues with dependencies.

### Step 6

Run `grails vaadin-quickstart` that will generate sample code to easier project configuration.

![Vaadin plugin](http://vaadinongrails.com/book/1_1_2_quickstart.png)

The command `vaadin-quickstart` will generate `MyUI.groovy` and remove URL mapping inside `UrlMapping.groovy`.

##### Optional

We have to disable Grails to take control over the URLs, so Vaadin can do it instead. Open `UrlMappings.groovy` file and make sure the URL mapping is empty, so the content of the file is the following.

``` java
class UrlMappings {
    static mappings = {
    }
}
```

> If you see ```unable to resolve class com.vaadin.grails.VaadinConfiguration``` error during compile, compile the project once more. There an issue with Grails together with IntelliJ IDEA and it is not possible to compile the project in the first run. Note that everything will work properly and you can ingnore that error.

### Step 8

We are ready to run the application. Click on the little green triangle in toolbar.

![Compile app](http://vaadinongrails.com/img/run-app-idea.png)

> You can also press `Alt+Cmd+G` on Mac OS or `Ctrl+Alt+G` on Windows and type `run-app` in order to run the application.

### Step 9

Run the application again and a Vaadin application with a single Home label will be available on http://localhost:8080/ria-app in your browser.

![Running app](http://vaadinongrails.com/img/first-run.png)



