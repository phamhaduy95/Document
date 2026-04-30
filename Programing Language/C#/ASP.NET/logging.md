Logging

1.  **Introduction**

Beside Exception, logging is essential part for handling error as it
provides an effective way for diagnosing the problem. The logging
doesn't just record the detail about the error but chain of events which
may is the true cause for that error.

2.  **Logging in ASP.NET core**

To be able to work well with logging framework provided by ASP.NET core,
you should understand these concepts:

- Logging level: The log level of the log is how important it is and is
  defined by the LogLevel enum.

- ILogger: is the main interface which is used to injected into other
  service via DI to provide interface for logging message.

- Logging provider: is responsible for creating instance for ILogger
  object, it also specifics where the logging message go to. For
  example, ConsoleLoggerProvider has its loggers writing log message to
  the console terminal while EventLogLoggerProvider create the logger
  that write message to Window event log.

**2.1 Adding logging service to your application**

public class SampleController : ControllerBase

{

private readonly ILogger\<SampleController\> \_logger;

// adding logger service through DI

public SampleController(ILogger\<SampleController\> logger)

{

this.\_logger = logger;

}

public async Task\<IActionResult\> Index()

{

\_logger.LogInformation(\"enter Index action method\");

return Ok(\"index\");

}

}

**2.2 Logging level**

Whenever you create a log using ILogger, you must specify the log level.
This indicates how serious or important the log message is, and it's an
important factor when it comes to filtering which logs get written by a
provider, as well as finding the important log messages after the fact.

There are six log levels to choose from, ordered here from most to least
serious:

- *Critical*: For disastrous failures that may leave the app unable to
  function correctly, such as out-of-memory exceptions or if the hard
  drive is out of disk space or the server is on fire.

- *Error*: For errors and exceptions that you can't handle gracefully;
  for example, exceptions thrown when saving an edited entity in EF
  Core. The operation failed, but the app can continue to function for
  other requests and users.

- *Warning*---For when an unexpected or error condition arises that you
  can work around. You might log a Warning for handled exceptions, or
  when an entity isn't found.

- *Information*: For tracking normal application flow; for example,
  logging when a user logs in, or when they view a specific page in your
  app. Typically these log messages provide context when you need to
  understand the steps leading up to an error message.

- *Debug*: For tracking detailed information that's particularly useful
  during development.

- *Trace*: For tracking very detailed information, which may contain
  sensitive information like passwords or keys. It's rarely used, and
  not used at all by the framework libraries.

You can specific which level the logging should be, by selecting the
suitable method from the ILogger interface.

3.  **Logging provider**

Logging providers control the destination of your log messages in
ASP.NET Core. They take the messages you create using the ILogger
interface and write them to an output location, which varies depending
on the provider.

Microsoft has written several first-party log providers for ASP.NET Core
that are available out-of-the-box in ASP.NET Core. These include

Console provider---Writes messages to the console, as you've already
seen.

Debug provider---Writes messages to the debug window when you're
debugging an app in Visual Studio or Visual Studio Code, for example.

EventLog provider---Writes messages to the Windows Event Log. Only
outputs log messages when running on Windows, as it requires
Windows-specific APIs.

EventSource provider---Writes messages using Event Tracing for Windows
(ETW) or LTTng tracing on Linux.

2.4 Logging message

![](media/image1.png){width="4.789098862642169in"
height="1.6427919947506562in"}

The exact presentation of the message will vary depending on where the
log is written, but each log record includes up to six common elements:

- *Log level*: The log level of the log is how important it is and is
  defined by the LogLevel enum.

- *Event category*: The category may be any string value, but it's
  typically set to the name of the class creating the log. For
  ILogger\<T\>, the full name of the type T is the category.

- *Message*: This is the content of the log message. It can be a static
  string, or it can contain placeholders for variables, as shown in
  listing 17.2. Placeholders are indicated by braces, {}, and are
  substituted with the provided parameter values.

- *Parameters*: If the message contains placeholders, they're associated
  with the provided parameters.

- *Exception*: If an exception occurs, you can pass the exception object
  to the logging function along with the message and other parameters.
  The logger will log the exception, in addition to the message itself.

- *EventId*: This is an optional integer identifier for the error, which
  can be used to quickly find all similar logs in a series of log
  messages. You might use an EventId of 1000 when a user attempts to
  load a non-existent recipe, and an EventId of 1001 when a user
  attempts to access a recipe they don't have permission to access. If
  you don't provide an EventId, the value 0 will be used.
