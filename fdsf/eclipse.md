# Eclipse

> Working example project, which results from this tutorial, can be downloaded from [github.com/vaadin-on-grails/quickstart-app](https://github.com/vaadin-on-grails/quickstart-app).

Spring provides great tool [Groovy/Grails Tool Suite](http://spring.io/tools/ggts) for development of Groovy and Grails based projects. If Eclipse is your development IDE, you should definetelly get GGTS.

We will show how to create a new Grails project with Vaadin using GGTS in this article.

### Step 1

Search for Grails type of project in `New Project` wizard and click on `Next`.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_new-project.png)

### Step 2

Type a name of the project and click on `Finish`.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_project-name.png)

### Step 3

Open file `BuildConfig.groovy` and add Vaadin plugin `compile ":vaadin:7.3.3.2"`.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_vaadin.png)

> The latest version of the plugin is always available on http://grails.org/plugin/vaadin

Run `grails compile` to reload dependencies. Click on Grail icon in GGTS and type there `compile`.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_compile.png)

Then run `grails vaadin-quickstart` that will generate sample code to easier project configuration.

The command `vaadin-quickstart` will generate `MyUI.groovy` and remove URL mapping inside `UrlMapping.groovy`.

##### Optional

We have to disable Grails to take control over the URLs, so Vaadin can do it instead. Open `UrlMappings.groovy` file and make sure the URL mapping is empty, so the content of the file is the following.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_urlmapping.png)

### Step 6

Click on Grail icon again and run `run-app` command.

![Setup Grails](http://vaadinongrails.com/book/1_1_4_run-app.png)

### Step 7

Now your application is running on http://localhost:8080/my-app

![Setup Grails](http://vaadinongrails.com/book/1_1_4_running.png)

