# Create SAML Assertion using the API-Gateway

In cases where the API-Gateway is required to dynamically create a SAML Assertion
to be send to downstream applications. For instance, when a user has been authenticated
and a SAML Assertion on the behalf of the user must be created.

For this purpose, the standard filter: "Insert SAML Attribute Assertion" can be used,
but this filter requires some kind of special input parameters.  

The scripting filter creates the required attribute: `attribute.lookup.list` and can be
combined into a policy like so:  
![./images/sample-policy.png]

## Attribute: attribute.subject.id
The authenticated user the SAML-Assertion should belong too.   

Sample: sampleuser  

## Attribute: attribute.lookup.list
A list of attributes should be part of the assertion. In fact, that list must be a
`java.lang.Map<String, RetrievedAttribute>`.  
This basically looks like:  
` {Key123=key=[Key123] name=[Key123] values=[Value123] namespace=[##nonamespace##] namespaceForAssertion=[urn:vordel:attribute:1.0] useForAssertion=[true], Key0815=key=[Key0815] name=[Key0815] values=[Value0815] namespace=[##nonamespace##] namespaceForAssertion=[urn:vordel:attribute:1.0] useForAssertion=[true]}`

## Script-Examples
The following script is used to create the required attribute: `attribute.lookup.list`
and can be adjusted as needed.

### Javascript
```javascript
var imp = new JavaImporter(java.util, com.vordel.circuit.attribute);

with(imp) {

 function invoke(msg) {
   var userProperties = new java.util.HashMap();
   var list1 = new ArrayList();
   var list2 = new ArrayList();
   list1.add("Value123");
   list2.add("Value0815");

   var attribute1 = new RetrievedAttribute(6, "Key123", null, list1);
   var attribute2 = new RetrievedAttribute(6, "Key0815", null, list2);
   userProperties.put("Key123", attribute1);
   userProperties.put("Key0815", attribute2);
   msg.put("attribute.lookup.list", userProperties);
   return true;
 }
}
```

### Groovy
N/A

### Jython
N/A

## Changelog
- 0.0.1 - 29.01.2020
  - Initial version


## Limitations/Caveats
- N/A

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management-Plus/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"
