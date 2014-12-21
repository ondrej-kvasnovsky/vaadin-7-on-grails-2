# Without using root URL

> Example code is available on
[github.com/vaadin-on-grails/rest-without-root](https://github.com/vaadin-on-grails/rest-without-root).

If you need to provide REST API from your application and you do not want to provide that from other application, there is a way how to do it from Grails even if we run there Vaadin applications.

First thing if you want to have REST API, do not map any Vaadin UI on `/*` (root) url. If you have to use the root, check the next article "Using root URL for Vaadin app".

### Step 1

Your mapping should like like this.

``` java
mapping = [
    "/admin/*": "app.AdminUI",
    "/client/*": "app.ClientUI"
]
```

### Step 2

Then, we can add URL mapping into `UrlMappings.groovy`.

``` java
class UrlMappings {
	static mappings = {
		group "/rest", {
			"/$controller/$action?/$id?"()
		}
	}
}
```

### Step 3

Create a controller `grails create-controller app.ItemController` and implement some logic there. For example, fetch some data from database and return it as JSON. In our example, we will just return a map, with dummy data, as JSON.

``` java
package app

import grails.converters.JSON

class ItemController {

    def index() {
        Map data = ['some': 'data']

        return data as JSON
    }
}
```

## Step 4

Create two sample Vaadin UIs, so we can verify URL mapping.

``` java
package app

import com.vaadin.grails.ui.DefaultUI
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout

class ClientUI extends DefaultUI {

    protected void init(VaadinRequest request) {
        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Label label = new Label("Client")
        layout.addComponent(label)

        setContent(layout)
    }
}
```

``` java
package app

import com.vaadin.grails.ui.DefaultUI
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout

class AdminUI extends DefaultUI {

    protected void init(VaadinRequest request) {
        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Label label = new Label("Admin")
        layout.addComponent(label)

        setContent(layout)
    }
}
```


### Step 5

Start up the application and try out different URLs:
* http://localhost:8080/rest-without-root/client
* http://localhost:8080/rest-without-root/admin
* http://localhost:8080/rest-without-root/rest/item/index
