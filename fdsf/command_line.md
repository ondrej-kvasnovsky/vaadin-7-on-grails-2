# Command line

> Working example project, which results from this tutorial, can be downloaded from [github.com/vaadin-on-grails/quickstart-app](https://github.com/vaadin-on-grails/quickstart-app).

Using [Grails command line](http://grails.org/doc/latest/guide/single.html#commandLine) to create a new application is probably the most reliable way. We can basically say that if you are not able to create project using the command line, you will be not able to work with Grails at all and you need to re-install Grails or fix its configuration.

## Create application

These instructions are supposed to be executed by using the command line.

#### Step 1
Open your console and run `grails create-app my-app` that will create new directory called **my-app** with sample Grails application.

#### Step 2

Go to that directory `cd my-app` and open that folder in a text editor of your choice.

#### Step 3

Open `BuildConfig.groovy` and add latest version of [vaadin](http://grails.org/plugin/vaadin) plugin into compile scope `compile ":vaadin:{{version}}"`.

![BuildConfig.groovy](http://vaadinongrails.com/book/1_1_build_config.png)

#### Step 4

Now we need to run `grails vaadin-quickstart`, so Vaadin plugin can generate sample application for us.

![Generated Vaadin sample code](http://vaadinongrails.com/book/1_1_vaadin_sample_app.png)

#### Step 5

We want to prevent Grails to take over URL mapping. Open `UrlMappings.groovy` and make sure the URL mapping is empty.

``` java
    class UrlMappings {
        static mappings = {
        }
}
```

#### Step 6
We are ready to start up the application. Run grails run-app command. Vaadin application running on http://localhost:8080/my-app will become accessible after a while.
  ![Generated Vaadin sample code](http://vaadinongrails.com/book/1_1_run_app.png)


