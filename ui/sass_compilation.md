# SASS Compilation

> Example code is available on
[github.com/vaadin-on-grails/sass-compilation](https://github.com/vaadin-on-grails/sass-compilation).

SASS compilation will happen automatically, during runtime, in all environments except of production environment. This is handy, because we do not have to recompile SASS files after each change.

If automatic compilation of SASS is not happening, make sure `productionMode` is set to `false` for development environment, in `VaadinConfig.groovy`.

Because automatic re-compilation shoud not happen in production, because of performance, Vaadin plugin will compile SASS files for you when you build the production war file, as part of `grails prod war` command.

Let's create an example theme that will extend new Valo theme provided by Vaadin 7.3.


### Step 1

Create new folders `/VAADIN/themes/mytheme` in `web-app`. Then we create new SASS file named `styles.scss` in `web-app/VAADIN/themes/mytheme` folder.

``` java
$v-app-background-color: hsl(71, 100%, 87%);

@import "../valo/valo";

.mytheme {
  @include valo;
}
```

### Step 2

Add the name of a theme to `VaadinConfig.groovy`, in our example `mytheme`.

We can also define Vaadin version for SASS compilation by defining `sassCompile` variable. If we do not define Vaadin version a default version will be provided by Vaadin plugin.

``` java
vaadin {
    themes = ['mytheme']
    sassCompile = '7.3.0'
}
```

Then `styles.scss` gets compiled into `styles.css` when we run
`grails prod war` command.

> This configuration is required for SASS compilation that is executed as part of `grails prod war` command.

### Step 3

The last step is to let Vaadin know what is the active theme. Add `@Theme` annotation with name of our custom theme on our `UI` class.

``` java
package app

import com.vaadin.annotations.Theme
import com.vaadin.grails.Grails
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Button
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

@Theme("mytheme")
class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        String homeLabel = Grails.i18n("default.home.label")
        Button label = new Button(homeLabel)
        layout.addComponent(label)

        setContent(layout)
    }
}
```

### Step 4

Run the app and notice the changed background and the button, which is using Valo theme.

> When you add `?debug` as a parameter into URL, you wil see what theme is used as well as other debug logs.

![Valo theme customized](http://vaadinongrails.com/book/5_5_4_valo.png)


