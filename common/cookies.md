## Intro
Cookies is pretty simple concept
Server can use `Set-Cookie` to store key-value pair in client browser and then when the browser makes a request it attach that key-value pairs to the request those allow servers to has some info about the client. The problem is the security of those key-value pairs, since they may have some sensitive data that should be protected from being stolen by third-parties. There're other way to store some data on the client like: Web Storage API or IndexedDB, but they're never sent to server (implicitly at least)

## Lifetime
- Permanent
- Session

## Security

### Restrications
Restrict access to cookies

Use `Secure` attribute enforce that the response is sent only through HTTPS or HTTP but only for *localhost*. Which means [[main-in-the-middle]] attackers can't access it easily. But someone with access to the hard drive (or js if the `HttpOnly` is not set) can read and modify the information.

A cookie with the `HttpOnly` attribute is inaccessible to the JavaScript [`Document.cookie`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie) API; it's only sent to the server. For example, cookies that persist in server-side sessions don't need to be available to JavaScript and should have the `HttpOnly` attribute. This precaution helps mitigate cross-site scripting ([XSS](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#cross-site_scripting_(xss))) attacks.