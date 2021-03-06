# Widgetset Compilation

> Example code is available on
[github.com/vaadin-on-grails/widgetset-compilation](https://github.com/vaadin-on-grails/widgetset-compilation).

This article shows how to recompile widgetset when we add an add-on that requires widgetset recompilation.

We will demonstrate how to add [tokenfield](https://vaadin.com/directory#addon/tokenfield:vaadin). But you can use this tutorial in the same way for the other add-ons as well.

### Step 1

Add the depedencies to add-ons in `BuildConfig.groovy`.

``` java
repositories {
    inherits true

    grailsPlugins()
    grailsHome()
    mavenLocal()
    grailsCentral()
    mavenCentral()

    mavenRepo "http://maven.vaadin.com/vaadin-addons"
}

dependencies {
    compile "org.vaadin.addons:tokenfield:7.0.1"
}
```

### Step 2

Create `src/java/app/widgetset.gwt.xml` file with widgetset references.

``` java
<!DOCTYPE
    module PUBLIC "-//Google Inc.//DTD Google Web Toolkit 2.0//EN"
    "http://google-web-toolkit.googlecode.com/svn/releases/2.0/distro-source/core/src/gwt-module.dtd">
<module>
    <inherits name="org.vaadin.tokenfield.TokenfieldWidgetset"/>
</module>
```

> If you want to add more add-ons, just add more lines with widget sets.
```
<module>
    <inherits name="org.vaadin.tokenfield.TokenfieldWidgetset"/>
    <inherits name="org.vaadin.otheraddon1.TheWidgetset"/>
    <inherits name="org.vaadin.otheraddon2.TheWidgetset"/>
</module>
```

### Step 3

Add the widgetset reference into `VaadinConfig.groovy`.

``` java
vaadin {

    mapping = [
            "/*": "app.MyUI"
    ]

    productionMode = false

    widgetset = 'app.widgetset'
}

environments {
    production {
        vaadin {
            productionMode = true
        }
    }
}
```

We can try to run the application in order to see that the application will not come up. That will show that widgetset compilation is mandatory.

### Step 4

We will add a text field on UI. Use `TokenField` in order to finish this example.

``` java
package app

import com.vaadin.server.VaadinRequest
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout
import org.vaadin.tokenfield.TokenField

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        TokenField f = new TokenField("Add tags")
        layout.addComponent(f)

        setContent(layout)
    }
}
```

### Step 5

Run `grails vaadin-compile-widgetset --verbose` to recompile the widgetset.

### Step 6

After the widgetset compilation is done and you run the application, you will see the tokenfield component in the browser.

![Valo theme customized](http://vaadinongrails.com/book/5_5_5_widgetset.png)


