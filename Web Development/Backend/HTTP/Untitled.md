More interesting components are the user and password components. Many servers require a username and password before you can access data through them. FTP servers are a common example of this.
ftp://anonymous:my_passwd@ftp.prep.ai.mit.edu/pub/gnu


HTTP quy định các thành phần và cấu trúc của message 

The HTTP specifications explain HTTP messages fairly well

map mô tả quy trình trao đổi message giữa HTTP client và HTTP servers

![[Pasted image 20251124224819.png]]


Nagle 's algorithms

TCP has a data stream interface that permits applications to stream data of any size to the TCP stack—even a single byte at a time! But because each TCP segment carries at least 40 bytes of flags and headers, network performance can be degraded severely if TCP sends large numbers of packets containing small amounts of data.

persistent connection
 allows HTTP devices to keep TCP connections open after transactions complete and to reuse the preexisting connections for future HTTP requests



fleeting note
MD5 stands for ==Message-Digest Algorithm==, a cryptographic hash function that produces a 128-bit (32-character hexadecimal) hash value from an input of any length. It was designed to ensure data integrity by creating a unique fingerprint for a file, but it is no longer considered secure for preventing malicious attacks due to known vulnerabilities. While deprecated for security-critical applications like password storage, it is still widely used as a checksum to detect unintentional errors in data transmission and storage


Accept-Language: es

resource hint
https://html.spec.whatwg.org/#linkTypes


• Critical resources such as CSS and JavaScript should be discoverable as early as possible in the document.
• CSS should be delivered as early as possible to unblock rendering and JavaScript execution.
• Noncritical JavaScript should be deferred to avoid blocking DOM and CSSOM
construction.
• The HTML document is parsed incrementally by the parser; hence the document should be periodically flushed for best performance.

fork and exec