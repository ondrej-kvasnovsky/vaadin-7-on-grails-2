# Multiple application

In `grails-app/conf/VaadinConfig.groovy`, we can configure URL mapping to Vaadin UI classes. We can define multiple UIs to be accessible from one Grails application.

In this article we will create two new UIs that we will show under different URLs.

### Step 1

`ClientUI` will be available on http://localhost:8080/client.

``` java
package app

import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

class ClientUI extends UI {

    protected void init(VaadinRequest request) {
        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Label label = new Label("Client")
        layout.addComponent(label)

        setContent(layout)
    }
}
```

And `AdminUI` that will be mapped to http://localhost:8080/server

``` java
package app

import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

class AdminUI extends UI {

    protected void init(VaadinRequest request) {
        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Label label = new Label("Admin")
        layout.addComponent(label)

        setContent(layout)
    }
}
```

### Step 2

Open `VaadinConfig.groovy` and change the mapping, so the mapping points to the new UIs.

``` java
mapping = [
    "/client/*": "app.ClientUI",
    "/admin/*": "app.AdminUI"
]
```

Now we can start up the application and access both URLs to see each is mapped to different UI class.
