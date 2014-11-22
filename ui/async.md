# Async Push

Push has been added in [Vaadin plugin](http://grails.org/plugin/vaadin) version 7.3.0.1. Make sure you have this, or later, version of the plugin.

### Step 1

Enable anyc in VaadinConfig.groovy.

``` java
asyncSupported = true
```

### Step 2

In order to demonstrate Vaadin push, we will create a view that will create a label that will have initial value that will be overriden by a thread. The thread will wait one second and then set other text into the label. Let's say, that we replace waiting by a call to external system, where we expect a delay.

For pusing another label, we create `Pusher` thread that will change text of the label.

``` java
package app.views.push

import app.MyUI
import com.vaadin.navigator.View
import com.vaadin.navigator.ViewChangeListener
import com.vaadin.ui.Label
import com.vaadin.ui.VerticalLayout

class PushView extends VerticalLayout implements View {

    static final String VIEW_NAME = "push"

    Label loadingText = new Label("Loading UI, please wait...");

    @Override
    void enter(ViewChangeListener.ViewChangeEvent viewChangeEvent) {
        loadingText.setSizeUndefined();
        addComponent(loadingText);
        new Pusher().start();
    }

    class Pusher extends Thread {
        @Override
        public void run() {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }

            MyUI.current.access(new Runnable() {
                @Override
                public void run() {
                    loadingText.setValue("Done!")
                }
            });
        }
    }
}
```

