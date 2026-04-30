HttpClient and HttpClientFactory

1.  HttpClient

**1.1 introduction**

You can call web APIs inside your ASP.NET core web application to
interexchange data with server. ASP.NET provides us the HttpClient class
which represents the connection between two IP addresses using HTTP
protocol.

There are several reasons for calling APIs inside the back-end
application:

- The first is to acquire data from third party web APIs service.

- The second is to decouple your MVC or Razor Pages web app with main
  data server serving data through web API. The traditional approach is
  to build the MVC or Razor Pages on top of the data access layer in one
  single project for data query and modification. This create the
  dependency between database and view parts, which is not considered
  good practice. One feasible solution is to create the separated
  project for view using MVC or Razor Pages then fetching data through
  API calling mechanism.

- Lastly, when you need to prepare the unit test for testing web API.

**1.2 using HttpClient**

To establish the HTTP connection, we initiate the HttpClient object.

async Task fetchData()

{

using (HttpClient client = new HttpClient())

{

/\*\* implement HttpClient here \*/

}

}

Since the HttpClient uses system resource, it is essential to free any
used resource by deposing the HttpClient object after you finish using
it. The using statement is the great automatic solution for this.
Moreover, most HttpClient functions are asynchronous, we should
encapsulate the HttpClient execution within any async operation.

The example below illustrates how a typical HttpClient is used.

async Task\<string\> fetchData()

{

using (HttpClient client = new HttpClient())

{

// make HttpClient send the HTTP get request to targeted url.

var response = await
client.GetAsync(\"https://api.exchangeratesapi.io//latest\");

// it will throw the HttpRequestException if the response status code is
not valid.

response.EnsureSuccessStatusCode();

// wait the message finish parsing.

var message = await response.Content.ReadAsStringAsync();

return message;

}

}

**1.3 Creating Http Request with HttpClient**

HttpClient class consists of several methods for adjust the content and
behavior for all Http request that it makes. With the help of these
method, we can:

- Set the base target url for all request.

- Adding or modifying Http header.

- Select the appropriate Http method for the request.

- Prepare the content for body part.

a.  **Setting base URL**

httpClient.BaseAddress = new Uri(\"https://api.exchangeratesapi.io\");

var response = await client.GetAsync("latest\");

b.  **Adding or modifying Http header**

Use HttpClient.DefaultRequestHeaders to set the default header for all
request.

client.DefaultRequestHeaders.Add(\"new-header\", \"true\");

For a specific request only, you need to compose a separated Http
request message for that request.

using (var request = new HttpRequestMessage(HttpMethod.Get,\"url\")

{

request.Headers.Authorization = new
AuthenticationHeaderValue(\"Bearer\", Token);

var response = await HttpClient.SendAsync(request);

response.EnsureSuccessStatusCode();

return await response.Content.ReadAsStringAsync();

}

c.  **Selecting Http Method.**

HttpClient consists of several built-in interface to help us send a Http
request with a specific method.

HttpClient.DeleteAsync(): send delete HTTP request.

HttpClient.PutAsync(): send Put request.

HttpClient.GetAsync(): set Get request

Or you can construct your own HttpRequestMessage object which allows you
freely set the method type or modify header and content of a Http
Request. Then use HttpClient.SendAsync() to send new created message.

d.  **Composing the content for Http Request Body**

The best approach to do this is to instantiate new HttpRequestMessage
and add the content for it. For JSON media type, you need to change the
content-type header to "application/json" and use any JSON handler
library to serialize the content.

using (HttpClient client = new HttpClient())

{

var messageToSend = new HttpRequestMessage();

var Car = new

{

id = Guid.NewGuid(),

name = \"Mercedes\",

License = \"23234\",

};

// serialize C# object into json object notation.

var json = JsonConvert.SerializeObject(Car);

// StringContent is the class which compose the body content with
desired char coding and media type (application/json in this case)

var content = new
StringContent(json,System.Text.Encoding.UTF8,\"application/json\");

// use Post or Put method which use body to make the request

var response = await client.PostAsync(\"url\", content);

response.EnsureSuccessStatusCode();

var message = await response.Content.ReadAsStringAsync();

}

e.  **Adding query string to http request.**

As query string is an optional part of the url, you can append the query
string to the url using regular string operation. However, for
convention, you can opt to UriBuilder utility class for safely
constructing a correct url.

var u = \"http://webcode.me/qs.php\";

using var client = new HttpClient();

var builder = new UriBuilder(u);

builder.Query = \"name=John Doe&occupation=gardener\";

var url = builder.ToString();

var res = await client.GetAsync(url);

var content = await res.Content.ReadAsStringAsync();

Console.WriteLine(content);

**1.4 Reading the HttpClient response result**

**1.5 the flaws of HttpClient**

HttpClient class is good enough for handling a handful of simple http
requests. However, it is not suitable for large-scale back-end
application due to these inheriting shortcomings:

- Firstly, since each HttpClient object, when it is initiated, take some
  system resource which is specifically the connection port. As the more
  Http requests coming the more HttpClient object is required, the port
  of your system will be soon used up. This scenario is often called
  *port exhaustion*.

- The other problematic issue is

To avoid these problems, ASP.NET gives us the HttpClientFactory which is
designed for efficiently managing the HttpClient object creation.

2.  HttpClientFactory

As mentioned in previous section, The ASP.NET provides us a
HttpClientFactory whose purpose is to efficiently manage the
initialization and life time of HttpClient object. Apart from its
built-in powerful HttpClient lifetime management, using
HttpClientFactory also bring upfront some noticeable benefits.

The first one is the single configuration place in program.cs which
allows user to configure some important aspect and behaviors of
HttpClient object.

To implement HttpClientFactory in your project, the first thing to do is
making HttpClient configuration in program.cs of your ASP.NET app. There
two main strategies to configurate the HttpClientFactory.

- using Name client pattern

- using typed client pattern

  1.  Using Name client pattern

In this section you'll learn how to use the Named Client pattern with
IHttpClientFactory. This pattern encapsulates the logic for calling a
third-party API in a single location, making it easier to use the
HttpClient in your consuming code. From main entry program "program.cs",
you can register a HttpClient with its name and configuration.

// register HttpClient named \"student\" and provides default
configuration.

builder.Services.AddHttpClient(\"students\", client =\>

{

client.BaseAddress = new Uri(\"https://https://localhost:5001\");

client.DefaultRequestHeaders.Add(HeaderNames.UserAgent,
\"C#-testing-StudentAPI\");

});

Then you can inject the IHttpClientFactory into the service you want to
implement HttpClient. Using CreateClient with the name of targeted
HttpClient configuration.

public class StudentClientService : IStudentClientService

{

private readonly IHttpClientFactory \_clientFactory;

public StudentClientService(IHttpClientFactory clientFactory)

{

\_clientFactory = clientFactory;

}

public async Task\<StudentModel\> GetStudentById(Guid Id)

{

//request HttpClientFactory to instantiate the HttpClient object through
its name

var client = \_clientFactory.CreateClient(\"students\");

var repsonse = await client.GetAsync(\$\"{Id}\");

repsonse.EnsureSuccessStatusCode();

var content = await repsonse.Content.ReadAsStringAsync();

var student = JsonConvert.DeserializeObject\<StudentModel\>(content);

return student;

}
