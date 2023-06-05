# Intro
Rate limiter is used in networking to manage the traffic in the network
When we talk about HTTP we can use rate limiter to limit the number of API calls a user can do per minute, hour, etc. If user do more API calls than he can, he won't be able to access API until conditions is satisfied (number of API calls reset) 

# Use Cases
- Prevent DoS-attacks
- Billing reduction
- To prevent servers from overloading

# Implementation

Where should rate limiter reside?
Answering the question we can divide rate limiter into server-side and client-side.

**Client-side rate limiter** can be used, but it is not reliable since can be easily disabled, but there are some use cases for it.

**Server-side rate limiter.**
Can be places in a server or in an API Gateway.


Algorithms for rate limiting:
- Token bucket
- Leaking bucket
- Fixed window counter
- Sliding window log
- Sliding window counter

## Token bucket
We have a bucket. There are limited number of tokens in each bucket. When user do request token is discarded, if bucket is empty, user cannot access the API.

What about number of buckets?
It depends.
We can attach each user a bucket, or attach a bucket per API route, depends on the logic.
For example if we talk about Chat GPT v3.5 (at the time of writing this) we can use 3 call per minute, so we need only one bucket for user.

Pros:
- easy to implement
- memory management
Cons:
- finding the right value for the max number of tokens, and time when it should be reset

## Leaking bucket
Similar to the previous one, but bucket is not reset. Instead tokens is added as time passes.
The algorithm is defined by two arguments **size of the bucket**, **the speed of the token refilling (units = token/second)**

## Fixed window counter
## Sliding window log
## Sliding window counter

# Architecture

![[Pasted image 20230601220932.png]]

Sticky session
: Sticky session is a session which enforce a load balancer to forward massages from the client to the same server
