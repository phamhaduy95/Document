Provide user-friendly interface to encrypt and decrypt string data.

Basically, protecting data consists of the following steps:

1.  Create a data protector from a data protection provider.

2.  Call the Protect method with the data you want to protect.

3.  Call the Unprotect method with the data you want to turn back into
    plain text.

using System;

using Microsoft.AspNetCore.DataProtection;

using Microsoft.Extensions.DependencyInjection;

public class Program

{

public static void Main(string\[\] args)

{

// add data protection services

var serviceCollection = new ServiceCollection();

serviceCollection.AddDataProtection();

var services = serviceCollection.BuildServiceProvider();

// create an instance of MyClass using the service provider

var instance = ActivatorUtilities.CreateInstance\<MyClass\>(services);

instance.RunSample();

}

public class MyClass

{

IDataProtector \_protector;

// the \'provider\' parameter is provided by DI

public MyClass(IDataProtectionProvider provider)

{

\_protector = provider.CreateProtector(\"Contoso.MyClass.v1\");

}

public void RunSample()

{

Console.Write(\"Enter input: \");

string input = Console.ReadLine();

// protect the payload

string protectedPayload = \_protector.Protect(input);

Console.WriteLine(\$\"Protect returned: {protectedPayload}\");

// unprotect the payload

string unprotectedPayload = \_protector.Unprotect(protectedPayload);

Console.WriteLine(\$\"Unprotect returned: {unprotectedPayload}\");

}

}

}

/\*

\* SAMPLE OUTPUT

\*

\* Enter input: Hello world!

\* Protect returned: CfDJ8ICcgQwZZhlAlTZT\...OdfH66i1PnGmpCR5e441xQ

\* Unprotect returned: Hello world!

\*/

When you create a protector you must provide one or more [Purpose
Strings](https://learn.microsoft.com/en-us/aspnet/core/security/data-protection/consumer-apis/purpose-strings?view=aspnetcore-6.0).
A purpose string provides isolation between consumers.

 For example, a protector created with a purpose string of \"green\"
wouldn\'t be able to unprotect data provided by a protector with a
purpose of \"purple\".

Components which consume IDataProtectionProvider must pass a
unique *purposes* parameter to the CreateProtector method. The
purposes *parameter* is inherent to the security of the data protection
system, as it provides isolation between cryptographic consumers, even
if the root cryptographic keys are the same

A better purposes chain for the messaging component would
be CreateProtector(\[ \"Contoso.Messaging.SecureMessage\", \$\"User:
{username}\" \]), which provides proper isolation.

**PersistKeysToDbContext**

builder.Services.AddDataProtection()

.PersistKeysToDbContext\<SampleDbContext\>();

The preceding code stores the keys in the configured database. The
database context being used must
implement IDataProtectionKeyContext. IDataProtectionKeyContext exposes
the property DataProtectionKeys

public DbSet\<DataProtectionKey\> DataProtectionKeys { get; set; } =
null!;

builder.Services.AddDataProtection()

.SetDefaultKeyLifetime(TimeSpan.FromDays(14));

When hosting in
a [[Docker]{.underline}](https://learn.microsoft.com/en-us/dotnet/standard/microservices-architecture/container-docker-introduction/) container,
keys should be maintained in either:

- A folder that\'s a Docker volume that persists beyond the container\'s
  lifetime, such as a shared volume or a host-mounted volume.

- An external provider, such as [[Azure Blob
  Storage]{.underline}](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction) (shown
  in
  the [ProtectKeysWithAzureKeyVault](https://learn.microsoft.com/en-us/aspnet/core/security/data-protection/configuration/overview?view=aspnetcore-6.0#protectkeyswithazurekeyvault) section)
  or [[Redis]{.underline}](https://redis.io/).

Understand the key lifetime

The key is stored within the keyring which is the collection of all keys
generated

 That said, there\'s nothing prohibiting a developer from using the
ASP.NET Core data protection APIs for long-term protection of
confidential data. Keys are never removed from the key ring,
so IDataProtector.Unprotect can always recover existing payloads as long
as the keys are available and valid

However, an issue arises when the developer tries to unprotect data that
has been protected with a revoked key, as IDataProtector.Unprotect will
throw an exception in this case.
