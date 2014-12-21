# Re-using GORM Validations

> Example code is available on
[github.com/vaadin-on-grails/gorm-validations](https://github.com/vaadin-on-grails/gorm-validations).

When we define validation rules, constraints, in domain class we probably do not want to repeat that validation rules in Vaadin code. We would rather re-use it in Vaadin code for validation of input fields.

We can get constraints from a domain class and use it. Because Vaadin provides many validator, we can use them and fill it with what we have defined in `constraints` for a domain class.

Assume we have a product, domain class, that has a name. The product name needs to fulfil certiain validation rules. The name cannot be blank, it has too be more than 2 characters long and maximum lenght is 255.

``` java
class Product {

    String name

    static constraints = {
        name(blank: false, minSize: 2, maxSize: 255)
    }
}
```

Then we can get the constraints in Vaadin code and use it as parameters for validation.

``` java
Product product = new Product()
product.name = "iPhone 7"
product.save()

FormLayout form = new FormLayout()
FieldGroup binder = new FieldGroup(new BeanItem(product))

TextField txnName = binder.buildAndBind("Name", "name")
txnName.setNullRepresentation("")

def constraints = Product.constraints
def name = constraints.name

int minSize = name.minSize
int maxSize = name.maxSize
boolean blank = name.blank

StringLengthValidator validator = new StringLengthValidator("Validation failed", minSize, maxSize, blank)
txnName.addValidator(validator)

form.addComponent(txnName)
```
