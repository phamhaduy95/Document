*the necessary of HTTPS*

security issue of regular HTTP: By default, HTTP requests are
unencrypted; they're plain text files being sent over the internet.
Anyone on the same network as a user (such as someone using the same
public Wi-Fi in a coffee shop) can read the requests and responses sent
back and forth. Attackers can even modify the requests or responses as
they're in transit.

To protect your users, your app should encrypt the traffic between the
user's browser and your app as it travels over the network by using the
HTTPS protocol. This is similar to HTTP traffic, but it uses an SSL/TLS1
certificate to encrypt requests and responses, so attackers cannot read
or modify the contents.

*Adding HTTPS to an application*
