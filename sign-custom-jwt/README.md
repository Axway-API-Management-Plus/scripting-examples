# Create a signed JWT with custom fields

The standard Axway API-Gateway filter: "JWT Sign" doesn't support custom JWT-Header fields. The
default JWT created by the filter looks like this depending on the selected algorithm:
```json
{
"alg":"RS512"
}
```
If you would like to create a JWT with a header like so:
```json
{
"type":"jwt",
"alg":"RS512",
"kid":"3122132"
}
```
you need to use a scripting filter as shown below.

### Sample Policy ###
![Sample-Policy](./images/sample-policy.png)

__Set JWT-Payload__  
![Sample-Policy](./images/set-jwt-payload.png)

__Set Required Attributes__  
![Sample-Policy](./images/set-attributes.png)

__Trace Output__
The Trace-Output shows the following JWT-Payload:  
```
jwt {
     Value: eyJraWQiOiIxMjM0NTY3ODkiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.ew0KICAic3ViIjogIjEyMzQ1Njc4OTAiLA0KICAibmFtZSI6ICJKb2huIERvZSIsDQogICJpYXQiOiAxNTE2MjM5MDIyDQp9.lg6Fhz12DgIXAp9kLjNXPLvsZ8BitCs2N3l1ILDs9nS8r8GNyoFZFkkYUJvtGtdGBjrte-bYB6xzRTnnPPf-QqL84wKea94OehSBjnoZE2lfXDWpev1Qep3hiPbDeM-B0p3xCnVumP6u9xSEyiwaV06CTNOG1Ot-g7jprWB_oCNaMzPDacbX8nY3ZeiL5ykebmgAHHaGuKvb4MTg6wvcB0Qrg_TqsYzzwZlJd6ERqtdbmPzQC3uf3L988CKbOUyCoFhQcgq7aBxuWBlZMeWOygqXhrlejTO6hlvYvmrf0AqsXqqpS1WBqapeSFdmpaFpW9vigGYgha3Qy4KbxKBMdg
     Type: java.lang.String
}
```

## Script-Examples

You need to provide the following attributes:  

| Attribute&nbsp;Name | Description | Example |
| ----------- | --------- | ------------- |
| **algorithm** | The Signing-Alghorythm to use. Possible values are: [RS256, RS384, RS512, PS256, PS384, PS512] | RS256 |
|**kid**|The Key-ID parameter|78678678687687|
|**jwtPayload**|An attribute containing the JWT-Payload to sign.|`{"sub": "1234567890","name": "John Doe","iat": 1516239022}`|
|**certificateAlias**|The certificate containing the private key to sign the JWT|CN=Change this for production|

You may extend the script code as you need in case you need to add any other customer header.

### Javascript
```javascript
var imp = new JavaImporter(com.nimbusds.jose,com.nimbusds.jose.crypto, com.vordel.store.cert);
with(imp) {
	function invoke(msg) {
		var algorithm = msg.get("algorithm");
		var kid = msg.get("kid");
		var jwtPayload = msg.get("jwtPayload");
		var certificateAlias = msg.get("certificateAlias");
		var signAlgorithm = JWSAlgorithm.parse(algorithm);

		var privateKey = CertStore.getInstance().getPersonalInfoByAlias(certificateAlias).privateKey;
		var signer = new RSASSASigner(privateKey);
		var builder = new JWSHeader.Builder(signAlgorithm);
		builder.type(JOSEObjectType.JWT);
		builder.keyID(kid);
		var jwsHeader = builder.build();
		var  payload = new Payload(jwtPayload);
		var jwsObject = new JWSObject(jwsHeader, payload);
		jwsObject.sign(signer)
		msg.put("jwt", jwsObject.serialize());
		return true;
	}
}
```

### Groovy
N/A

### Jython
N/A

## Changelog
- 0.0.1 - 30.01.2020
  - Initial version


## Limitations/Caveats
- N/A

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management-Plus/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"
