# Using root URL for Vaadin app

> Example code is available on
[github.com/vaadin-on-grails/rest-with-root](https://github.com/vaadin-on-grails/rest-with-root).

URL mapping can be done also in way we use `/` root URL to display Vaadin application. In this article, we are going to show how to do it.

### Step 1

Create mapping in `UrlMapping.groovy` as follows:

``` java
class UrlMappings {

    static mappings = {
        "/" {
            controller = "redirect"
        }
        // add your URL mapping for controllers
        group "/rest", {
            "/$controller/$action?/$id?"()
        }
    }
}
```

### Step 2

Then we need to create the controller that will redirect the request to the Vaadin application.

``` java
package app

class RedirectController {

    def index() {
        redirect(uri: "/app")
    }
}
```

### Step 3

Last step is to make sure that VaadinConfig.groovy contains mapping to Vaadin UI class.

``` java
vaadin {
    mapping = [
            "/app/*": "app.MyUI"
    ]
}
```

### Step 4

Create a controller and implement some logic there. For example, fetch some data from database and return it as JSON.

``` java
package app

class RedirectController {

    def index() {
        redirect(uri: "/app")
    }
}
```

### Step 5

Create two sample Vaadin UI, so we can verify URL mapping.

``` java
package app

import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.grails.Grails

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        String homeLabel = Grails.i18n("default.home.label")
        Label label = new Label(homeLabel)
        layout.addComponent(label)

        setContent(layout)
    }
}
```

### Step 6

Start up the application and try out these URLs:

* http://localhost:8080/rest-with-root redirects user to application
* http://localhost:8080/rest-with-root/rest/item displays JSON response
