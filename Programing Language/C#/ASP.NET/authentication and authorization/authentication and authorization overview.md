# Authentication and Authorization Overview

Security in ASP.NET Core is built on two primary processes: **Authentication** and **Authorization**. These processes work together to secure your application's resources.

- **Authentication**: The process of determining a user's identity ("Who are you?").
- **Authorization**: The process of determining whether a user has access to a specific resource ("What are you allowed to do?").

---

## 1. Middleware Pipeline Order
For security to function correctly, the middleware must be registered in the following sequence in `Program.cs`:

1.  **`app.UseRouting()`**: Matches the request to an endpoint.
2.  **`app.UseAuthentication()`**: Identifies the user based on credentials (e.g., a cookie or token).
3.  **`app.UseAuthorization()`**: Checks if the identified user has permission to access the matched endpoint.
4.  **`app.Map...()`**: Executes the endpoint.

---

## 2. The Authentication Phase
Handled by the **Authentication Middleware**, this phase involves verifying credentials and establishing the user's identity.

1.  **Credentials**: The client provides an identifier (e.g., email) and a secret (e.g., password).
2.  **Verification**: The app verifies these against a secure store (like ASP.NET Core Identity).
3.  **Claims**: If valid, the app generates a set of **Claims** (statements about the user, like their Name or Role).
4.  **Persistence**: These claims are stored in an **Authentication Cookie** or a **JWT Token** and sent back to the client for future requests.

---

## 3. The Authorization Phase
Handled by the **Authorization Middleware**, this phase checks if the authenticated user meets the requirements for the requested resource.

### Types of Authorization Responses:
- **Success**: The user is authenticated and has the required permissions. The request proceeds to the endpoint.
- **Challenge (401 Unauthorized)**: The user is not authenticated. The app "challenges" them to provide credentials (e.g., by redirecting to a login page).
- **Forbidden (403 Forbidden)**: The user is authenticated but does **not** have the necessary rights (e.g., a "Customer" trying to access an "Admin" dashboard).

---

## 4. Common Security Schemes
- **Cookie Authentication**: Primarily used for traditional web apps with browsers.
- **JWT Bearer Authentication**: The standard for modern APIs and Single Page Applications (SPAs).
- **ASP.NET Core Identity**: A full-featured membership system that handles users, passwords, roles, and two-factor authentication.
