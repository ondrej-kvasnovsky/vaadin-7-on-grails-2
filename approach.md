# Using root URL for Vaadin app

URL mapping can be done also in way we use '/' root URL to display Vaadin application.

### Step 1

Create mapping in `UrlMapping.groovy` as follows:

``` java
	static mappings = {
	    "/"{
            controller = "redirect"

        }
        // add your URL mapping to controllers
    }
```

### Step 2

Then we need to create the controller that will redirect the request to the Vaadin application.

``` java
class RedirectController {

    def index = {
        redirect(uri: "/vaadin")
    }
}
```

### Step 3

Last step is to make sure that VaadinConfig.groovy contains mapping to Vaadin UI class.

``` java
vaadin {
    mapping = [
            "/vaadin/*": "com.myapp.MyUi"
    ]
}
```
