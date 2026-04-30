SignalR uses *hubs* to communicate between clients and servers.

A hub is a high-level pipeline that allows a client and server to call
methods on each other. SignalR handles the dispatching across machine
boundaries automatically, allowing clients to call methods on the server
and vice versa.

You can pass strongly-typed parameters to methods, which enables model
binding. SignalR provides two built-in hub protocols: a text protocol
based on JSON and a binary protocol based
on [MessagePack](https://msgpack.org/).

**Configure SignalR hubs**

o register the services required by SignalR hubs,
call [AddSignalR](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in Program.cs:

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();

builder.Services.AddSignalR();

To configure SignalR endpoints,
call [MapHub](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub),
also in Program.cs

app.MapRazorPages();

app.MapHub\<ChatHub\>(\"/Chat\");

app.Run();

Hubs
are [**[transient]{.underline}**](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#transient):

- Don\'t store state in a property of the hub class. Each hub method
  call is executed on a new hub instance.

- Use await when calling asynchronous methods that depend on the hub
  staying alive. For example, a method such
  as Clients.All.SendAsync(\...) can fail if it\'s called
  without await and the hub method completes before SendAsync finishes.

The [Hub](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hub) class
includes
a [Context](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hub.context) property
that contains the following properties with information about the
connection.

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Property**                                                                                                                                      **Description**
  ------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------
  [[ConnectionId]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.connectionid)             Gets the unique ID for the connection, assigned by SignalR. There\'s one connection ID for each connection.

  [[UserIdentifier]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.useridentifier)         Gets the [[user identifier]{.underline}](https://learn.microsoft.com/en-us/aspnet/core/signalr/groups?view=aspnetcore-6.0). By default, SignalR
                                                                                                                                                    uses
                                                                                                                                                    the [[ClaimTypes.NameIdentifier]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/system.security.claims.claimtypes.nameidentifier) from
                                                                                                                                                    the [[ClaimsPrincipal]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/system.security.claims.claimsprincipal) associated with the
                                                                                                                                                    connection as the user identifier.

  [[User]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.user)                             Gets the [[ClaimsPrincipal]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/system.security.claims.claimsprincipal) associated with the
                                                                                                                                                    current user.

  [[Items]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.items)                           Gets a key/value collection that can be used to share data within the scope of this connection. Data can be stored in this collection and it will
                                                                                                                                                    persist for the connection across different hub method invocations.

  [[Features]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.features)                     Gets the collection of features available on the connection. For now, this collection isn\'t needed in most scenarios, so it isn\'t documented in
                                                                                                                                                    detail yet.

  [[ConnectionAborted]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.connectionaborted)   Gets a [[CancellationToken]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/system.threading.cancellationtoken) that notifies when the
                                                                                                                                                    connection is aborted.
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

[[Hub.Context]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hub.context) also
contains the following methods:

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  **Method**                                                                                                                                          **Description**
  --------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------------------
  [[GetHttpContext]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.gethttpcontextextensions.gethttpcontext)   Returns
                                                                                                                                                      the [[HttpContext]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.http.httpcontext) for
                                                                                                                                                      the connection, or null if the connection isn\'t associated with an HTTP request. For HTTP connections, use this method
                                                                                                                                                      to get information such as HTTP headers and query strings.

  [[Abort]{.underline}](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.signalr.hubcallercontext.abort)                             Aborts the connection.
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Send messages from outside a hub

The SignalR hub is the core abstraction for sending messages to clients
connected to the SignalR server. It\'s also possible to send messages
from other places in your app using the IHubContext service.

In ASP.NET Core SignalR, you can access an instance of IHubContext via
dependency injection. You can inject an instance of IHubContext into a
controller, middleware, or other DI service. Use the instance to send
messages to clients.

The IHubContext is for sending notifications to clients, it is not used
to call methods on the Hub.

##  Users in SignalR

A single user in SignalR can have multiple connections to an app. For
example, a user could be connected on their desktop as well as their
phone. Each device has a separate SignalR connection, but they\'re all
associated with the same user. If a message is sent to the user, all of
the connections associated with that user receive the message. The user
identifier for a connection can be accessed by
the Context.UserIdentifier property in the hub.

By default, SignalR uses the ClaimTypes.NameIdentifier from
the ClaimsPrincipal associated with the connection as the user
identifier

Client index.ts

import \* as signalR from \"@microsoft/signalr\";

import \"./css/main.css\";

const divMessages: HTMLDivElement =
document.querySelector(\"#divMessages\");

const tbMessage: HTMLInputElement =
document.querySelector(\"#tbMessage\");

const btnSend: HTMLButtonElement = document.querySelector(\"#btnSend\");

const username = new Date().getTime();

const connection = new signalR.HubConnectionBuilder()

.withUrl(\"/hub\")

.build();

connection.on(\"messageReceived\", (username: string, message: string)
=\> {

const m = document.createElement(\"div\");

m.innerHTML = \`\<div
class=\"message-author\"\>\${username}\</div\>\<div\>\${message}\</div\>\`;

divMessages.appendChild(m);

divMessages.scrollTop = divMessages.scrollHeight;

});

connection.start().catch((err) =\> document.write(err));

tbMessage.addEventListener(\"keyup\", (e: KeyboardEvent) =\> {

if (e.key === \"Enter\") {

send();

}

});

btnSend.addEventListener(\"click\", send);

function send() {

connection.send(\"newMessage\", username, tbMessage.value)

.then(() =\> (tbMessage.value = \"\"));

}
