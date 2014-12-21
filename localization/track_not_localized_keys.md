# Custom MessageSource

> Example code is available on
[github.com/vaadin-on-grails/custom-messagesource](https://github.com/vaadin-on-grails/custom-messagesource).

We might want to define a custom `MessageSource` that will return a localized message. If a message is missing, we might want to log information about missing localization (in logs or database) or we might want to provide other way to fetch the localized labels.

In this toturial we will show way to load localization from database.

### Step 1

Enable OSIV in `VaadinConfig.groovy`. Add or comment out the following line.

``` java
openSessionInViewFilter = 'org.springframework.orm.hibernate4.support.OpenSessionInViewFilter'
```

### Step 2

Create new domain object that will hold localization data. Run `grails create-domain app.Message` command.

* `key` holds the localization key that we will you in our application to refer a localized string
* `value` is localized string, for example a label in English
* `locale` defines in what language is `value` string

``` java
package app

class Message {

    String key
    String value
    Locale locale
}
```

### Step 3

Create localized key-value pair for given language in `BootStrap.groovy`.

``` java
import app.Message

class BootStrap {

    def init = { servletContext ->

        Message home = new Message(key: 'home', value: 'Home', locale: Locale.ENGLISH)
        home.save(failOnError: true)
    }
    def destroy = {
    }
}
```

### Step 4

Implement a new message source that will use GORM domain object, `Message` in our case, to load the localized messages. When a value is not found, we will return the key in brackets.

``` java
package app.i18n

import app.Message
import org.codehaus.groovy.grails.context.support.PluginAwareResourceBundleMessageSource
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.context.support.AbstractMessageSource

import java.text.MessageFormat

class JdbcMessageSource extends AbstractMessageSource {

    @Autowired
    PluginAwareResourceBundleMessageSource messageBundleMessageSource

    @Override
    protected MessageFormat resolveCode(String code, Locale l) {
        Message message = Message.findByKeyAndLocale(code, l)

        MessageFormat format
        if (message) {
            format = new MessageFormat(message.value, message.locale)
        } else {
            format = messageBundleMessageSource.resolveCode(code, l)
            if (!format) {
                format = new MessageFormat("[$code]", l)
            }
        }
        return format
    }
}
```

### Step 4

Open `grails-app/conf/spring/resources.groovy` and add a new bean `messageSource` and `messageBundleMessageSource` that we use to get the localization in case the localization is not found in database.

``` java
import app.i18n.JdbcMessageSource
import org.codehaus.groovy.grails.context.support.PluginAwareResourceBundleMessageSource

beans = {

    messageSource(JdbcMessageSource)

    messageBundleMessageSource(PluginAwareResourceBundleMessageSource) {
        basenames = "WEB-INF/grails-app/i18n/messages"
    }
}
```

### Step 5

Now we can use standard `Grails.i18` method to get localized strings.

``` java
package app

import com.vaadin.grails.Grails
import com.vaadin.server.VaadinRequest
import com.vaadin.ui.Label
import com.vaadin.ui.UI
import com.vaadin.ui.VerticalLayout

class MyUI extends UI {

    @Override
    protected void init(VaadinRequest r) {

        VerticalLayout layout = new VerticalLayout()
        layout.setMargin(true)

        Label lbl1 = new Label(Grails.i18n('do.not.exist', Locale.ENGLISH))
        layout.addComponent(new Label(lbl1))

        Label lbl2 = new Label(Grails.i18n('home', Locale.ENGLISH))
        layout.addComponent(new Label(lbl2))

        setContent(layout)
    }
}
```

### Step 6

Run the application `grails run-app` and open it in the browser http://localhost:8080/custom-messagesource. Then we will see the messages in the console.

![Login screen](http://vaadinongrails.com/book/7_2_custom-messagesource.png)

