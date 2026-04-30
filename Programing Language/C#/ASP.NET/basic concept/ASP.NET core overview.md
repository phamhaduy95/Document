# ASP.NET Core Overview

ASP.NET Core is a modern, open-source, cross-platform web development framework developed and maintained by Microsoft. It is a complete overhaul of the legacy .NET Framework, designed to be faster, more modular, and easier to maintain.

## Key Advantages
- **Cross-Platform**: Develop and run applications on Windows, macOS, and Linux.
- **High Performance**: Optimized for modern cloud and server environments.
- **Unified Story**: A single framework for building Web UI (Razor Pages/MVC) and Web APIs.
- **Modular**: Feature-rich but only includes the dependencies you actually need via NuGet packages.

---

## Core Application Patterns
Most ASP.NET Core applications follow one of three main patterns:

### 1. Web Application (UI)
Designed for direct use by humans in a browser. It typically uses **Razor Pages** or **MVC** to generate dynamic HTML content. It handles form submissions, navigation, and user interaction.

### 2. Web API
Designed for consumption by other machines, mobile apps, or Single Page Application (SPA) frameworks (like React or Angular). These applications typically exchange data in **JSON** or **XML** formats rather than HTML.

### 3. Hybrid Applications
An application can serve both HTML pages for browsers and JSON APIs for client-side code, allowing for a flexible architecture that shares underlying business logic.

---

## The Middleware Pipeline
The heart of an ASP.NET Core application is the **Middleware Pipeline**. Every request that enters the application passes through a series of components (middleware) that can handle, modify, or pass the request further down the chain. This modularity allows you to easily add features like authentication, logging, and static file serving by simply adding them to the pipeline in `Program.cs`.
