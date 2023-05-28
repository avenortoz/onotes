### Desc
JSON web token (JWT), pronounced "jot", is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Again, JWT is a standard, meaning that all JWTs are tokens, but not all tokens are JWTs.

Because of its relatively small size, a JWT can be sent through a URL, through a POST parameter, or inside an HTTP header, and it is transmitted quickly. A JWT contains all the required information about an entity to avoid querying a database more than once. The recipient of a JWT also does not need to call a server to validate the token.

Advantages:
- More compact: JSON is less verbose than XML, so when it is encoded, a JWT is smaller than a SAML token. This makes JWT a good choice to be passed in HTML and HTTP envs
- More secure(questionable)
- More common(depends on the domain, but since it is simple it is used often in simple apps)
- Easier to process
Drawbacks:
- [TODO]

### Structure
A well-formed JWT consists of three concatenated Base64url-encoded strings, separated by dots (`.`):

- **JOSE Header**: contains metadata about the type of token and the cryptographic algorithms used to secure its contents.
    
- **JWS payload** (set of [claims](https://tools.ietf.org/html/rfc7519#section-4)): contains verifiable security statements, such as the identity of the user and the permissions they are allowed.
    
- **JWS signature**: used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you **must** [check its signature](https://auth0.com/docs/secure/tokens/json-web-tokens/validate-json-web-tokens) before storing and using it.


#### JOSE header

JSON object containing the parameters describing the cryptographic operations and parameters employed. The JOSE (JSON Object Signing and Encryption) Header is comprised of a set of Header Parameters that typically consist of a name/value pair: the hashing algorithm being used (e.g., HMAC SHA256 or RSA) and the type of the JWT.

```json
{
      "alg": "HS256",
      "typ": "JWT"
}
```

#### JWS payload

The payload contains statements about the entity (typically, the user) and additional entity attributes, which are called claims. In this example, our entity is a user.

```json
{
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true
}
```

#### JWS signature

The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

To create the signature, the Base64-encoded header and payload are taken, along with a secret, and signed with the algorithm specified in the header.

For example, if you are creating a signature for a token using the HMAC SHA256 algorithm, you would do the following:

```json
HMACSHA256(
      base64UrlEncode(header) + "." +
      base64UrlEncode(payload),
      secret)
```

#### Example
Example in [[rust]] how to create a JWT token:
```
use jsonwebtoken::{encode, Header, Algorithm, EncodingKey};

// This is the data that you want to include in the payload of the JWT token
let my_claims = claims {
    sub: "1234567890",
    name: "John Doe",
    iat: 1516239022,
};

// Encode the JWT key with a secret key
let my_secret = "secret";
let my_secret_key = EncodingKey::from_secret(my_secret.as_ref());

// Set the header with the JWT algorithm to use
let my_header = Header::new(Algorithm::HS256);

// Create the JWT token with the header and claims
let token = encode(&my_header, &my_claims, &my_secret_key)?;

// Print the JWT token
println!("Token: {}", token);
```
Example in [[react]] how to extract info from JWT token using *jsonwebtoken* npm package:
```
const payload = jwt.decode(token);
console.log(payload.name);
```
**Note**: it is not a good practice to place all info you need into single jwt token, it is a good way to keep balance, and make instead a request to the server if you need some more info

### Claims
There're two types of claims registered and custom claims.
First one is predefined like: *iss*, *sub*, *exp*, *iat* they can be used later for purposes like token expiration, role defining, and are commonly exploited by frameworks to simplify use. For example you can enforce expiration policy or is signing key is correct, etc.

Custom claims can be used to store required data.
They consists of non-registered public or private claims. Public claims are collision-resistant while private claims are subject to possible collisions.

### HTTP header
`Authorization: Bearer <token>`

### Token storage
The best place to store jwt token is secure cookie. Local storage probably the worst when it comes to security, cause any script can access the data and steal it.