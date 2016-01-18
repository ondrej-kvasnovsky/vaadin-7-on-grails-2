# Table Container

> Example code is available on
[github.com/vaadin-on-grails/gorm-open-session-in-view-ii](https://github.com/vaadin-on-grails/gorm-open-session-in-view-ii).

In the previous article we have described how to enable OSIV in VaadinConfig.groovy. This tutorial will show way how to do it in the old way, without configuration.

The other way to use OSIV in Grails with Vaadin is to manually add an extra filter `OpenSessionInViewFilter` into `web.xml` file.

### Step 1