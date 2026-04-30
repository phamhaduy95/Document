Handling JSON request

It is not common for us to handle the HTTP request in JSON format file.
One of the high-performance and popular libraries to handling JSON in
.NET environment is NewtonSoft.Json packet. The packet offers some
useful benefits and feature

- Flexible JSON serializer for converting between .NET objects and JSON.

- LINQ to JSON for manually reading and writing JSON.

- High performance: faster than .NET\'s built-in JSON serializers.

- Write indented, easy-to-read JSON.

- Convert JSON to and from XML.

you can install this packet through NuGet Packet management and import
it at the beginning of the file.

using NewtonSoft.Json;

Serializing and deserializing the JSON

For simple conversion from .NET object to JSON string and vice versa,
you can use SerializeObject() and DeSerializeObject\<T\>() from the
JsonConvert class. For example

using NewtonSoft.Json;

Product product = new Product();

product.Name = \"Apple\";

product.ExpiryDate = new DateTime(2008, 12, 28);

product.Price = 3.99M;

product.Sizes = new string\[\] { \"Small\", \"Medium\", \"Large\" };

string output = JsonConvert.SerializeObject(product);

//{

// \"Name\": \"Apple\",

// \"ExpiryDate\": \"2008-12-28T00:00:00\",

// \"Price\": 3.99,

// \"Sizes\": \[

// \"Small\",

// \"Medium\",

// \"Large\"

// \]

//}

Product deserializedProduct =
JsonConvert.DeserializeObject\<Product\>(output);

For more control over the serializing and deserializing process,

For more guide of using NewtonSoft.Json library, you can read the
official document at https://www.newtonsoft.com/json/help/html
