# How to logout when using [[JWT]] for authentication

_1) Simply remove the token from the client_

Obviously this does nothing for server side security, but it does stop an attacker by removing the token from existence (ie. they would have to have stolen the token prior to logout).

_2) Create a token blocklist_

You could store the invalid tokens until their initial expiry date, and compare them against incoming requests. This seems to negate the reason for going fully token based in the first place though, as you would need to touch the database for every request. The storage size would likely be lower though, as you would only need to store tokens that were between logout & expiry time (this is a gut feeling, and is definitely dependent on context).

_3) Just keep token expiry times short and rotate them often_

If you keep the token expiry times at short enough intervals, and have the running client keep track and request updates when necessary, number 1 would effectively work as a complete logout system. The problem with this method, is that it makes it impossible to keep the user logged in between closes of the client code (depending on how long you make the expiry interval).

_Contingency Plans_

If there ever was an emergency, or a user token was compromised, one thing you could do is allow the user to change an underlying user lookup ID with their login credentials. This would render all associated tokens invalid, as the associated user would no longer be able to be found.

Link: https://stackoverflow.com/questions/21978658/invalidating-json-web-tokens/23089839#23089839