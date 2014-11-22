# Best Practices

## Keep work with GORM in Services

Never work with GORM in Vaadin code, use services or any other layer, like DAO, to encapsulate work with database. It will keep your Vaadin and database code separated, which will make the application code more readable and maintainable.

## CompileStatic

Use `@CompileStatic` annotation everywhere you can. The exception not to use the annotation is dynamic code where you need to utilize dynamic features of Grails, for example, to access variables that are not visible during compilation.

## Types

Use `def` only when it is required, because it always good to know type of a variable. Together with `@CompileStatic` annotation, you will know about compilation errors during compilation, not during runtime.
