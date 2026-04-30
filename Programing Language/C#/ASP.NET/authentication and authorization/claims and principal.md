Claim and Principal

The authentication middleware needs to provide some valid identifiers
for describing one specific user so that the authorization middleware
can make the decision about whether it make challenge or forbid response
for any request from that user. The user identifiers can be anything
from user's personal information such as username, password, email,
phone number or permission flags such as CanUseResource, CanEnterGate.

in ASP.NET core, a user is represented by a ClaimsPrincipal object. Each
user can have multiple identities, which are represented by
ClaimsIdentity objects. An identity contains one or more pieces of
information about the user, each of which is represented by a Claim.

List\<Claim\> claims = new List\<Claim\>(){

new Claim(ClaimTypes.Name, user.UserName),

new Claim(\"PhoneNumber\", user.PhoneNumber),

new Claim(ClaimTypes.Email, user.Email),

new Claim(\"Id\", user.Id),

};

// add lists of Claim object to ClaimIdentity

var claimIdentity = new ClaimsIdentity(claims);

the ClaimsPrincipal
