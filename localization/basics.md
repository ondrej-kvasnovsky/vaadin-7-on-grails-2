# Basics

Localization property files are stored in `grails-app/i18n` folder.

![Localization Folder](http://vaadinongrails.com/img/idea-i18n.png)

In order to access the values in from the localized property files, use `Grails.i18n()` method. `Grails` class is provided by the plugin.
Here are various ways how to use i18n method.

``` java
import static com.vaadin.grails.Grails.i18n
import static java.util.Locale.ENGLISH

String label = i18n("default.home.label")

String homeEng = i18n("default.home.label", ENGLISH)

Object[] newItemArgs = ["Proper label to be shown"]
String newItem = i18n("default.home.label", newItemArgs)

String newItemEng = i18n("default.home.label", newItemArgs, ENGLISH)

String newItemEngDef = i18n("do.not.exist", newItemArgs, "Default label for {0}", ENGLISH)
```
