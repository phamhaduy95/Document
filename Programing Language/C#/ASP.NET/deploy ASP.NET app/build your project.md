# Building and Publishing your Project

Before deploying your ASP.NET Core application to a production server, you must compile and package it using the `dotnet` CLI or Visual Studio. This process prepares the application for a high-performance environment.

---

## 1. Running in Development
During development, you can use the `run` command to quickly build and start your application.

```bash
dotnet run
```

---

## 2. Publishing for Production
The **`publish`** command compiles the application, resolves its dependencies, and produces the final set of files needed for deployment.

```bash
dotnet publish -c Release -o ./publish
```

### Key Parameters:
- **`-c Release`**: Always use the **Release** configuration for deployment. This ensures the compiler generates optimized code, removes debug symbols, and significantly improves application performance.
- **`-o [path]`**: Defines the output directory for the published files.

---

## 3. Choosing a Deployment Mode

### Framework-Dependent Deployment (FDD)
This is the standard mode. The application relies on a shared version of the .NET Runtime being pre-installed on the server.
- **Advantage**: Very small deployment size (only your code and dependencies).
- **Requirement**: The server must have the compatible .NET Runtime installed.

### Self-Contained Deployment (SCD)
In this mode, the .NET Runtime itself is included in the published folder.
- **Advantage**: The app can run on a server that doesn't have .NET installed. This provides "copy-and-paste" portability.
- **Requirement**: You must specify the target Runtime Identifier (e.g., `-r win-x64` or `-r linux-x64`).

---

## 4. Performance Optimization: ReadyToRun (RTR)
You can further improve the startup time of your application by using **ReadyToRun** compilation. This ahead-of-time (AOT) compilation reduces the work the Just-In-Time (JIT) compiler needs to do when the app starts.

```bash
dotnet publish -c Release -r win-x64 --self-contained true /p:PublishReadyToRun=true
```

> [!TIP]
> After publishing, your target folder will contain a `.dll` file named after your project. On the server, you can start your app by running: `dotnet YourProjectName.dll`.
