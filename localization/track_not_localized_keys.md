# Custom MessageSource

We might want to define a custom `MessageSource` that will return a localized message. If a message is missing, we might want to log information about missing localization (in logs or database) or we might want to provide other way to fetch the localized labels.

### Step 1

Create a message source that will use instead of `Grails.i18n` method to load localized messages. When a value is not found, we will log a message.

``` java
package app.i18n

import com.vaadin.grails.Grails
import org.springframework.context.MessageSource
import org.springframework.context.MessageSourceResolvable
import org.springframework.context.NoSuchMessageException

class MyMessageSource implements MessageSource {

    @Override
    String getMessage(String code, Object[] args, String defaultMessage, Locale locale) {
        String message
        try {
            message = Grails.applicationContext.getBean('messageSource').getMessage(code, args, locale)
        } catch (NoSuchMessageException e) {
            // TODO: add your handling of missing message for given code
            println "Missing localized value for '$code' key."
        }
        if (!message) {
            message = defaultMessage
        }

        return message
    }

    @Override
    String getMessage(String code, Object[] args, Locale locale) throws NoSuchMessageException {
        String message
        try {
            message = Grails.applicationContext.getBean('messageSource').getMessage(code, args, locale)
        } catch (NoSuchMessageException e) {
            // TODO: add your handling of missing message for given code
            println "Missing localized value for '$code' key."
        }

        return message
    }

    @Override
    String getMessage(MessageSourceResolvable resolvable, Locale locale) throws NoSuchMessageException {
        String code = resolvable.codes[0]
        String message
        try {
            message = Grails.applicationContext.getBean('messageSource').getMessage(code, resolvable.arguments, locale)
        } catch (NoSuchMessageException e) {
            // TODO: add your handling of missing message for given code
            println "Missing localized value for '$code' key."
        }

        return message
    }
}
```

### Step 2

Add a new bean `myMessageSource` that we use to get the localization.

``` java
import app.i18n.MyMessageSource

beans = {
    myMessageSource(MyMessageSource)
}
```

### Step 3

Then we get `myMessageSource` method and try to get the message that does not exist.

``` java
MyMessageSource messageSource = Grails.get('myMessageSource')
Label lbl = new Label(messageSource.getMessage('do.not.exist', null, Locale.ENGLISH))

```

Then we will see a message in the console.

```
Missing value for 'do.not.exist'
```
