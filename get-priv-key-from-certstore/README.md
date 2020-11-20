# Get a private key from the API-Gateway Certificate store

If you need a private key within you API-Gateway policy you can obtain it with the following script.

## Script-Examples

You need to provide the following attributes:  

| Attribute&nbsp;Name | Description | Example |
| ----------- | --------- | ------------- |
| **algorithm** | The Signing-Alghorythm to use. Possible values are: [RS256, RS384, RS512, PS256, PS384, PS512] | RS256 |
|**kid**|The Key-ID parameter|78678678687687|
|**jwtPayload**|An attribute containing the JWT-Payload to sign.|`{"sub": "1234567890","name": "John Doe","iat": 1516239022}`|
|**certificateAlias**|The certificate containing the private key to sign the JWT|CN=Change this for production|

You may extend the script code as you need in case you need to add any other custom header.

### Javascript
```javascript
var imp = new JavaImporter(com.vordel.es.EntityStoreFactory, com.vordel.es.util.ShorthandKeyFinder, com.vordel.es.EntityStore, com.vordel.es.Entity, com.vordel.store.cert.CertStore);
with(imp) {
	function invoke(msg) {
		var es = EntityStoreFactory().getInstance();
		var keyFinder = new ShorthandKeyFinder(es);
		var certEntity = keyFinder.getEntity("/[Certificates]name=Certificate Store/[Certificate]dname=Samples Test Certificate");

		var key = CertStore.getPrivateKey(certEntity, null);

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
