đinh nghĩa của hypermedia
The hypermedia strategy always has the same goal. Hypermedia is a way for the server to tell the client what HTTP requests the client might want to make in the future. It’s a menu, provided by the server, from which the client is free to choose. The server knows what might happen, but the client decides what actually happens.


• The <a> tag describes a GET request for one specific URL, which is made only if the user triggers the control.
• The <img> tag describes a GET request for one specific URL, which happens automatically, in the background.
• The <form> tag with method="POST" describes a POST request to one specific URL, with a custom entity-body constructed by the client. The request is only made if the user triggers the control.
• The <form> tag with method="GET" describes a GET request to a custom URL constructed by the client. The request is only made if the user triggers the control.
