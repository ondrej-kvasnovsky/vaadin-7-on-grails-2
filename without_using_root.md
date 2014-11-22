# Without using root URL

If you need to provide REST API from your application and you do not want to provide that from other application, there is a way how to do it from Grails even if we run there Vaadin applications.

First thing if you want to have REST API, do not map any Vaadin UI on `/*` (root) url.

Your mapping should like like this.

    mapping = [
        "/admin/*": "app.AdminUI",
        "/client/*": "app.ClientUI"
    ]

Then, you need to add URL mapping into `UrlMappings.groovy`.

    class UrlMappings {
    	static mappings = {
    		group "/rest", {
    			"/$controller/$action?/$id?"()
    		}
    	}
    }

Create a controller and implement some logic there. For example, fetch some data from database and return it as JSON.

