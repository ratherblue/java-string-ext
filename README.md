#String Externalization

Brief overview of string externalization using JSP and Spring MVC.

## Where?

Message properties are normally stored in `src/main/webapp/WEB-INF/messages`. 

If you add a new properties file, it will need to be added to the `ReloadableResourceBundleMessageSource` config in your project and your server restarted before the changes will be picked up.

## Examples

To implement strings in a JSP, use the [message tag included in the Spring tag library](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/spring.tld.html#spring.tld.message):

```jsp
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
```
  

### Basic example

Properties:
```
example-string.help-number = 1-800-555-5555
```

JSP:
```jsp
<spring:message code="example-string.help-number" />
```

Output:
```
1-800-555-5555
```

### Arguments

#### One argument

Properties:
```
example-string.copyright = © 2013–{0} Example Company, Inc.
```

JSP:
```jsp
<%-- get the "currentYear" from the backend --%>
<spring:message code="example-string.copyright" arguments="${currentYear}" />
```

Output:
```
© 2013–2014 Example Company, Inc.
```

#### Multiple arguments

Multiple arguments can be passed as a comma-separated list. If you have an argument that contains a comma, you can specify the `argumentSeparator` attribute and use a different delimeter. 

Properties:
```
example-string.help-number = 1-800-555-5555
example-string.need-help = Having problems? Please <a href="{0}">contact us online</a> \
    or call <a class="phone-link" href="tel:{1}">{1}</a>.
```

JSP:
```jsp
<spring:message var="helpNumber" code="example-string.help-number" />

<%-- default --%>
<spring:message code="example-string.need-help" arguments="/help,${helpNumber}" />

<%-- with argumentSeparator --%>
<spring:message code="example-string.need-help" arguments="/help|${helpNumber}" argumentSeparator="|" />
```

Output:
```jsp
Having problems? Please <a href="/shared/help/feedback/feedback.html">contact us online</a> or 
call <a class="phone-link" href="tel:1-800-426-4840">1-800-426-4840</a>.
```
   
### Handling plurals

Properties:
```
example-string.cart-items = You have {0} {0,choice,0#items|1#item|1<items} in your cart
```

JSP:
```jsp
<spring:message code="example-string.cart-items" arguments="${0}" />
<spring:message code="example-string.cart-items" arguments="${1}" />
<spring:message code="example-string.cart-items" arguments="${2}" />
<spring:message code="example-string.cart-items" arguments="${1500}" />
```

Output:
```
You have 0 items in your cart
You have 1 item in your cart
You have 2 items in your cart
You have 1,500 items in your cart
```

### Formatting numbers

Properties:
```
example-string.cost-total = Total: {0,choice,0#FREE|0<{0,number,currency}}
```

JSP:
```jsp
<spring:message code="example-string.cost-total" arguments="${0}" />
<spring:message code="example-string.cost-total" arguments="${.32}" />
<spring:message code="example-string.cost-total" arguments="${21}" />
<spring:message code="example-string.cost-total" arguments="${100.91}" />
```

Output:
```
Total: FREE
Total: $0.32
Total: $21.00
Total: $100.91
```

## Caveats

### Single quotes and parameters

If a string includes parameters **and** single quotes you have to escape the single quote with another single quote:

```
example-string.foo = Je m'appelle Foo
example-string.bar = Je m'appelle {0}
example-string.baz = Je m''appelle {0}
```

```jsp
<spring:message code="example-string.foo" />
<spring:message code="example-string.bar" arguments="Bar" />
<spring:message code="example-string.baz" arguments="Baz" />
```

Output:
```
Je m'appelle Foo
Je mappelle {0}
Je m'appelle Baz
```

### Newlines

If you are editing a string and want to create a newline to make it easier to read, you need to add a backslash to the end of the line.

Example:
```
example-string.lines = Line 1 \
  Line 2 \
  Line 3
```   
