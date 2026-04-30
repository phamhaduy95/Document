CONFIGURATION

This document will demonstrate you:

- How to add configuration to your app through various sources (json,
  environment variables, user secrets).

- Using strong typed object IOptions to obtain defined configuration
  settings in controllers or page handlers.

1.  **What is configuration**

Configuration is the set of external parameters provided to an
application that controls the application's behavior in some way. There
are two types of important data that may be contained inside
configuration file: A *setting* is any value that changes the behavior
of your application. A *secret* is a special type of setting that
contains sensitive data, such as a password, an API key for a
third-party service, or a connection string.

The configuration settings are often stored in external file such as
JSON, XML, \... There are some convincing reasons for this:

- firstly, letting configuration stored within the ASP.NET app may lead
  to some unnecessary but expensive recompilation for your whole
  program. So, it is better to keep these setting on handful static
  files.

- secondly, it is not common to public your code in public version
  control service such as GitHub, putting the secret in any static file
  may expose some sensitive data such as API key or password.

2.  **Config default settings for ASP.NET application**

ASP.NET allows some initial configuration in program startup phase. the
WebApplicationBuilder.Host consist of several properties for providing
default configurations for our app. Some important ones are.

UseContentRoot: This tells the application in which directory it can
find any configuration or view files it will need later.

ConfigureAppConfiguration: It's where you load the settings and secrets
for your app, whether they're in JSON files environment variables, or
command-line arguments.

ConfigureHostingConfiguration: is where your application determines
which Hosting environment it's currently running in.

ConfigureLogging: is where you can specify the logging settings for your
application.

var builder = WebApplication.CreateBuilder(args);

builder.Host.UseContentRoot(Directory.GetCurrentDirectory()) // specific
the location where the configuration file is in

.ConfigureHostConfiguration(host =\>

{

// config hosting setting

host.AddCommandLine(args);

}).ConfigureAppConfiguration((configBuilder) =\>

{

// config app setting which includes external setting.

}).ConfigureLogging(args =\>

{

//config logging setting

});

3.  **Adding configuration to your app**

ConfigureAppConfiguration method is the place where you can provide
default setup for your ASP.NET app. The ConfigureAppConfiguration
requires lambda expression whose ConfigurationBuilder is one of
arguments.

the ConfigurationBuilder type comprises variety of methods to acquire
setting data from any configuration providers, which are the sources
holding the configuration data. Some providers which we frequently
integrate in our project:

- *JSON file providers*: Loads settings from an optional JSON file
  called appsettings.json.

- *User secrets*: Loads secrets that are stored safely during
  development.

- *Environment variables*: Loads environment variables as configuration
  variables. These are great for storing secrets in production.

- *Command-line variables*: Uses values passed as arguments when you run
  your app.

The Build command from ConfigurationBuilder is used to gather all
settings from multiple configuration providers into one
IConfigurationRoot object.

The ASP.NET core, by default, initiates the ConfigurationBuilder object
for you and expose it as the input argument of
ConfigureAppConfiguration, which allows users to submit their personal
initial settings for the App. Then, Build command is called
automatically to generated the ConfigurationRoot object which can be
injected to the app 's controller and services so that user can access
the configuration data.

builder.Host.ConfigureAppConfiguration(configBuilder =\>

{

configBuilder.AddJsonFile(\"appsettings.json\", true);

configBuilder.AddCommandLine(args);

configBuilder.AddEnvironmentVariables();

});

4.  **Overriding configuration**

When there are similar key-value pair which is contained within two or
more configuration providers, only the value from the last provider will
be used, all previous value will be overridden.

For instance, the "SharedSetting" key-value exists in both
appsettings.json and appsettings.shared.json file. However, since the
appsettings.json is added to the configuration builder later, the value
of the "ShareSetting" on the file will be remained.

appsettings.shared.json

{

"SharedSetting": 1,

}

appsettings.json

{

"SharedSetting":3,

}

program.cs

builder.Host.ConfigureAppConfiguration(configBuilder =\>

{

configBuilder.AddJsonFile(\"appsettings.shared.json\", true);

configBuilder.AddJsonFile(\"appsettings.json\", true);

});

5.  **Acquiring configuration setting in your project**

The ASP.NET app automatically inject configuration IConfigurationRoot
object that is created from ConfigureAppConfiguration method into
IConfiguration service which is one of handful of default services in
ASP.NET. To access the configuration through dependency injection, you
add the IConfiguration dependency into the class that need access to
configuration data.

For example, suppose we have appsettings.json which contains MySetting
key-value.

{

\"Logging\": {

\"LogLevel\": {

\"Default\": \"Information\",

\"Microsoft.AspNetCore\": \"Warning\"

}

},

\"MySetting\": {

\"Setting1\": \"12qa\",

\"Setting2\": \"125a\",

\"Setting3\": \"1256\",

}

\"AllowedHosts\": \"\*\"

}

Then we inject the IConfiguration into the Controller.

public class HomeController1: Controller {

private readonly IConfiguration \_config;

public HomeController1( IConfiguration config){

\_config = config;

}

public IActionResult Index () {

string setting1 = \_config\[\"Setting:Setting1\"\];

string setting2 = \_config\[\"Setting:Setting2\"\];

return View();

}

}

6.  **Storing configuration secrets safely**

As soon as you build a nontrivial app, you'll find you have a need to
store some sort of sensitive data as a setting somewhere. This could be
a password, a connection string, or an API key for a remote service.

**6.1 using environment variables**

Storing these data inside appsettings.json is not good idea as when your
project is published on GitHub, these data can be seen and used for bad
purpose. One of the simple and safe places for storing secrets is
environment variables.

"Example here"

The environment variable is the great place to store configuration for
production mode. However, it is not suitable for development mode as
there may be many projects which want to have their own play to hide
secrets. For development mode, you can use User Secrets Manager instead.

2.  **using User Secret Manager**

The idea behind User Secrets is to simplify storing per-app secrets
outside of your app's project tree. This is similar to environment
variables, but you use a unique key for each app to keep the secrets
segregated. This is similar to environment variables, but you use a
unique key for each app to keep the secrets segregated.

User Secret Manager requires some setup to be usable:

- Open secrets.json file for User Secrets Manager, right click at the
  project root and choose manage user secret. In the .csproj file, the
  unique Id will be given as the string in the tag \<UserSecretsId\>.

- Add some key-value pair that you think its value need security.

- Supply the unique ID generated in .csproj file to
  ConfigurationBuilder.AddUserSecrets inside the scope
  Host.ConfigureAppConfiguration method. The lambda expression accepted
  in ConfigureAppConfiguration contains HostBuilderContext object as the
  first argument. HostBuilderContext is used to get the information
  about hosting environment.

builder.Host.ConfigureAppConfiguration((context, configBuilder) =\>

{

// add configuration from JSON file

configBuilder.AddJsonFile(\"appsettings.json\", true);

if (context.HostingEnvironment.IsDevelopment())

{

configBuilder.AddUserSecrets(\"1eaf5b30-b6f7-4bae-a6a2-9917779b1c8a\");

}

});

7.  **Using strongly typed settings with the options pattern**

Earlier in the document, we use Configuration\["key"\] syntax for
accessing the configuration setting. This approach requires correct
string value for key, which is messy and cumbersome. ASP.NET promotes
using strong typed settings as a better alternative for accessing
configuration data. To use this feature, install and import
Microsoft.Extensions.Options in your project.

*Note: "the last wins" rule still applies to the option pattern.*

ASP.NET core automatically gather all settings and binds them to strong
object type class Options. Then user can access them through that
Options object without any concern about writing string value.

For example. Suppose we have some setting defined in appsettings.json.

{

\"MySetting\": {

\"Setting1\": \"12qa\",

\"Setting2\": \"125a\",

\"Setting3\": \"1256\",

},

}

1.  Create class which represents the structure of MySetting.

public class MySetting{

public string Setting1 { get; set; }

public string Setting2 { get; set; }

public string Setting3 { get; set; }

}

2.  Register the service for binding MySetting class to the "MySetting"
    in appsettings.json in program.cs

> builder.Services.Configure\<MySetting\>(builder.Configuration.GetSection(\"MySettings\"));

3.  This last step lets you inject your options classes into controllers
    and services by injecting IOptions\<T\>

public class HomeController1 : Controller {

private readonly MySetting \_mySettings;

public HomeController1(IOptions\<MySetting\> mySettings){

\_mySettings = mySettings.Value;

}

public IActionResult Index() {

string setting1 = \_mySettings.Setting1;

string setting2 = \_mySettings.Setting2;

string setting3 = \_mySettings.Setting3;

return View();

}

}

8.  **Designing valid option class.**

The class for binding configuration must:

- be non-abstract,

- have a default public constructor.

Then, all properties within the class must:

- be public.

- has a getter (no set-only)

- has a setter or a non-null value.
