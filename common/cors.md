CORS - (Cross-Origin Resource Sharing) is a mechanism that allows web servers to give permission to web browsers to access resources that a hosted on a different origin.
Main article: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS0

## Simple requests

GET, POST, HEAD; Content-Type: text/plain;
Detail conditions here:
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#examples_of_access_control_scenarios
![[Pasted image 20230528134931.png]]

Make an actual request, but may block response if Access-Control-Allow-Origin doesn't fit
## Preflighted requests
First do an OPTIONS request and receiving the response that satisfy the conditions do the actual request for the resource.

![[Pasted image 20230528134902.png]]

