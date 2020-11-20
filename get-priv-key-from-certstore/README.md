# Get a private key from the API-Gateway Certificate store

If you need a private key within you API-Gateway policy you can obtain it with the following script.

## Script-Examples

To get the Shorthand-Key you can use the ES-Explorer, search for the correct certificate and use the function get Shorthand key.

### Javascript
```javascript
var imp = new JavaImporter(com.vordel.dwe.Service, com.vordel.common.crypto.PasswordCipher, com.vordel.es.util.ShorthandKeyFinder, com.vordel.es.EntityStore, com.vordel.es.Entity, com.vordel.store.cert.CertStore);
with(imp) {
	function invoke(msg) {
		var es = Service.getInstance().getStore();
		var keyFinder = new ShorthandKeyFinder(es);
		var certEntity = keyFinder.getEntity("/[Certificates]name=Certificate Store/[Certificate]dname=Samples Test Certificate");

		var key = CertStore.getPrivateKey(certEntity, new PasswordCipher(""));

		msg.put("key", key);
		return true;
	}
}
```

### Groovy
N/A

### Jython
N/A

## Changelog
- 0.0.1 - 2020.11.20
  - Initial version


## Limitations/Caveats
- N/A

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management-Plus/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"
