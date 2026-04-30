app.post (\'/api/addProductID', (request,repsone) = \>{

const productList = request.body;

response.status(400).json({message:"success"});

}

1.  Understand the dependency injection

ASP.NET supports the dependency injection (DI) software design pattern,
which is a technique for achieving Inversion of Control (IoC) between
classes and their dependencies.

The ideal of DI is not new as it is one popular technique to promote the
D letter, which stands for dependency inversion, in the famous SOLID
principle.

**1.1 what is dependency**

A dependency is an object that another object depends on. For example:

IndexModel class depends on MyDependency to operate its OnGet method.

public class MyDependency

{

public void WriteMessage(string message){
Console.WriteLine(\$\"MyDependency.WriteMessage called.
Message:{message}\");

}

}

public class IndexModel: PageModel

{

private readonly MyDependency \_dependency = new MyDependency();

public void OnGet()

{

\_dependency.WriteMessage(\"IndexModel.OnGet\");

}

}

This dependency between IndexModel and MyDependency is problematic for
these reasons:

- Firstly, when you want to change the implementation of \_dependency
  with new type, the IndexModel must be modified.

- Secondly, if MyDependency contains other dependencies and is used
  across many classes, the code updating process will become complicated
  and unmanageable.

**1.2 Apply dependency injection**

To solve this issue, you can apply the built-in dependency injection
features in your ASP.NET app. The

- Instead of using concreate type as dependency, use Interface so that
  it can accept many classes that implement the Interface.

public interface IMyDependency {

public void WriteMessage (string message);

}

- Create reference of that Interface in that class. And add the
  constructor which requires one input argument whose type is the
  dependency Interface. This constructor is essential for DI to work.

public class IndexModel : PageModel

{

private readonly IMyDependency \_dependency;

public IndexModel (IMyDependency dependency) {

\_dependency = dependency

}

public void OnGet () {

\_dependency.WriteMessage(\"IndexModel.OnGet\");

}

}

- Create a concrete class implementing the IMyDependency interface.

public class MyDependency : IMyDependency

{

public void WriteMessage(string message){
Console.WriteLine(\$\"MyDependency.WriteMessage called.
Message:{message}\");

}

}

- In program entry file "program.sc". use any dependency register
  methods from Service.

builder.Services.AddScoped\<IMyDependency, MyDependency\> ();

In case you want to swap MyDependency with new implementation (for
example MyDependency2), you replace it in the AddScoped\<\> statement.

Note: As for simple demonstration, I name dependency class and interface
as MyDependency and IDependency. However, the ASP.NET specifics the
conventional name for the dependency class as Service.

**1.3 Chain of dependencies (dependency graph)**

It is not common for one dependency object to have other object as
dependency. The built-in DI container use the graph to depict the
dependent relation between each class and traverse through the graph to
inject concrete class to appropriate dependency.

For example, MyDependency2 type implementing IMyDependency interface
consists one dependency class IService.

public class MyDependency2: IMyDependency

{

private readonly IService \_service;

public MeDepedency2 (IService service){

\_service = servive;

}

public void WriteMessage(string message){
Console.WriteLine(\$\"MyDependency.WriteMessage called.
Message:{message}\");

}

}

To help program run smoothly, all dependency within the dependency graph
must be injected with proper concreated class.

builder.Services.AddScoped\<IMyDependency, MyDependency2\> ();

builder.Services.AddScoped \<IService, MyService\> ();

4.  **Registering services using objects and lambdas**

The dependency class must fulfill these requirements below for DI
container to initiate the instance from that class successfully.

- The class must be a concrete type.

- The class must only have a single "valid" constructor that the
  container can use.

- For a constructor to be "valid," all constructor arguments must be
  registered with the container or they must be arguments with a default
  value.

Any class outside these criteria are registered through lambda
expression supplied to register method. For example

public class Helper {

private string \_message;

private int \_number;

public Helper(string message, int number){

\_message = message;

\_number = number;

}

public string GenerateMessage(){

> return \$\"message generated from helper: {\_message} + {\_number}\"

}

}

public class MyDependency3: IMyDependency

{

private readonly Helper \_helper;

public MeDepedency2 (Helper helper){

\_helper = helper;

}

> public void WriteMessage(string message){
>
> string message = \_helper.GenerateMessage();
>
> Console.WriteLine(message);

}

}

As the Helper method required actual value of string and number for two
data members, the DI won't have enough information to properly initiate
an instance for Helper. You need to guide the DI container how to do
that through lambda expression.

I recommend using stateless class or interface to be dependency

**1.5 Registering services multiple times**

You can register multiple implementations of a service with the DI
container. However, the only last registered one will be executed.

For example, we registered for IMyDependency interface with three
distinguished implementations, last of which is MyDependency3.

builder.Services.AddScoped\<IMyDependency, MyDependency2\> ();

builder.Services.AddScoped\<IMyDependency, MyDependency\> ();

// MyDependency3 is the final registration for IMyDependency so it is
chosen to be executed.

builder.Services.AddScoped\<IMyDependency, MyDependency3\> ();

To execute all of implementation, instead of one single reference for
one dependency, we use IEnumerable\<\> to store a collection of many
references.

public class IndexModel : PageModel

{

private readonly IEnumerable\<IMyDependency\> \_dependencies;

public IndexModel(IEnumerable\<IMyDependency\> dependencies) {

\_dependencies = dependencies

}

public void OnGet(string message){

foreach (var dependency in \_dependencies){

dependency.WriteMessage(message);

> }

}

}

when OnGet was called, all of implementations that is registered for
IMyDependency will be executed following the order in which each
implementation is in.

1.  Dependency lifetime

In ASP.NET Core, you can specify three different lifetimes when
registering a service with the built-in container:

- Transient: Every time a service is requested, a new instance is
  created. Within the same request, the DI container can return many
  instances for one DI service. Transient lifetimes can result in a lot
  of objects being created, so they make the most sense for lightweight
  services with little or no state.

- Scoped: Within a scope, all requests for a service will give you the
  same object. For different scopes you'll get different objects. In
  ASP.NET Core, each web request gets its own scope. Database contexts
  and authentication services are common examples of services that
  should be scoped to a request---anything that you want to share across
  your services within a single request but that needs to change between
  requests.

- Singleton: You'll always get the same instance of the service, no
  matter which scope. Singletons are convenient for objects that need to
  be shared or that are immutable and expensive to create. For instance,
  a caching service should be a singleton, as all requests need to share
  it.
