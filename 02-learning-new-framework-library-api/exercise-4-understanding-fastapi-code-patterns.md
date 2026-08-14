# Exercise 4: Understanding FastAPI Code Patterns

## Objective

The purpose of this exercise was to use AI to understand more complex FastAPI code patterns. I focused mainly on dependency injection, the repository pattern, middleware, authentication and role-based access control.

Instead of only looking at whether the code works, I focused on understanding why the code was structured in a particular way and how the different parts work together.

---

## Part 1: Analysing Complex Code

### Repository Pattern

I learned that the Repository pattern is used to separate database-related operations from the rest of the application.

In the example, the `Repository` class contains operations such as retrieving an object by its ID and listing objects. The `UserRepository` then extends this pattern with functionality specific to users, such as finding a user by username.

This separation makes the application easier to maintain because database operations are kept in one place instead of being written repeatedly inside API endpoints.

### Generic[T]

The `Generic[T]` part of the repository was initially difficult for me to understand.

I learned that `T` is a type variable that allows the repository to work with different model types.

For example, the same basic repository structure could be used for users, products or other database models instead of creating completely separate repository classes for every model.

This makes the code more reusable and reduces duplication.

### Dependency Injection

I learned that FastAPI uses dependency injection to provide functions and resources that an endpoint needs.

The `Depends()` function tells FastAPI that a particular dependency should be provided automatically.

For example, the database session is provided through:

```python
db: AsyncSession = Depends(get_db)
The route does not have to create the database session itself. FastAPI calls get_db() and provides the result to the route.

The same idea is used for authentication, where get_current_user depends on the OAuth2 token and database session.

I understood dependency injection as a way of giving a function what it needs without making the function responsible for creating everything itself.

Role-Based Access Control

The example also uses role-based access control.

The requires_role() function creates a decorator that checks whether the current user has the required permission.

For the admin endpoint, the code checks whether the current user is a superuser.

If the user does not have the required permission, FastAPI returns a 403 Forbidden error.

This means authentication and authorization are handled separately. Authentication determines who the user is, while authorization determines what that user is allowed to do.

Part 2: Tracing Execution Flow

I traced what happens when a request is made to:

/admin/users/

The request first passes through the middleware.

The timing middleware records the start time, allows the request to continue and then adds the processing time to the response headers.

FastAPI then processes the dependencies required by the endpoint.

The database dependency creates a database session.

The authentication dependency receives the OAuth2 bearer token and attempts to decode the JWT.

The JWT contains the username, which is then used to find the corresponding user in the database.

If the token is invalid or the user cannot be found, the request is rejected with a 401 Unauthorized response.

If the user is authenticated, the role check is performed.

For the admin endpoint, the user must have the required administrator permissions. If the user does not have permission, the request receives a 403 Forbidden response.

If authentication and authorization succeed, the endpoint uses the UserRepository to retrieve the users from the database.

The users are returned using the UserSchema response model.

The response then passes back through the middleware, which adds the processing time before the response is sent to the client.

My simplified execution flow
Client request
      ↓
Timing middleware
      ↓
Database dependency
      ↓
Authentication dependency
      ↓
JWT validation
      ↓
Find current user
      ↓
Check admin permission
      ↓
UserRepository
      ↓
Retrieve users
      ↓
UserSchema response
      ↓
Timing middleware
      ↓
Response to client

Tracing the request in this order helped me understand how the different parts of the application connect instead of looking at each function separately.

Part 3: Simplifying Complex Concepts
asynccontextmanager and lifespan

I learned that asynccontextmanager can be used to manage actions that happen when an application starts and stops.

The lifespan function contains startup logic before the yield statement and shutdown logic after it.

A simplified example is:

from contextlib import asynccontextmanager


@asynccontextmanager
async def lifespan(app):
    print("Application starting")
    yield
    print("Application shutting down")

The code before yield runs when the application starts, while the code after yield runs when the application shuts down.

This can be useful for setting up and cleaning up resources used by an application.

Timing Middleware

I learned that middleware can process requests before they reach an endpoint and responses before they are returned to the client.

The timing middleware records the time before the request is processed and then calculates how long the request took after the endpoint has finished.

It then adds the processing time to the response headers.

This is useful for monitoring the performance of an API.

JWT Authentication

I learned that JWT authentication allows a server to issue a token after a user successfully logs in.

The basic flow I understood is:

The user provides login information.
The application checks the credentials.
If they are correct, the application creates a JWT.
The client sends the token with later requests.
FastAPI retrieves the token using the OAuth2 dependency.
The application decodes and validates the token.
The user associated with the token is retrieved.
The endpoint can then determine whether the user is allowed to access the resource.

The important thing I learned is that the token allows the application to identify the authenticated user without requiring the user to log in again for every request.

Part 4: Building Understanding Through Implementation
Adding a Logging System

The challenge was to add a simple logging system that records user actions such as logins and administrator actions.

I would follow the same patterns already present in the application.

First, I would create a database model for the log entry.

The model could contain information such as:

user ID
action
timestamp
description

I would then create a repository responsible for saving and retrieving log records.

A service layer could be used to contain the business logic for creating log entries.

The database session would continue to be provided through FastAPI dependency injection.

The existing authentication dependency could also be used to identify the user performing the action.

For example, when a user logs in successfully, the application could create a log entry such as:

User logged in

An administrator action could similarly be recorded with information about what the administrator did.

This approach follows the existing architecture because the database operations remain in the repository, business logic remains in the service layer and dependencies provide resources to the endpoints.

What I Learned

This exercise helped me understand that a larger FastAPI application can contain several layers that each have a specific responsibility.

The repository handles database operations.

The service layer handles business logic.

Dependencies provide resources and information to routes.

Middleware handles functionality that applies across requests and responses.

Authentication identifies the user, while authorization determines whether the user has permission to perform an action.

I also learned that generic classes can make code reusable instead of creating the same functionality repeatedly for different models.

Reflection

The most difficult part of this exercise was understanding how all the different patterns work together.

When I first looked at the code, there were many concepts happening at the same time, including generics, repositories, dependencies, middleware, JWT authentication and decorators.

Breaking the request into smaller steps made the code easier to understand.

Tracing the /admin/users/ request was particularly useful because I could see the order in which authentication, authorization, dependencies, repositories and the endpoint interact.

I also learned that I do not need to understand a complex application all at once. I can take one component at a time and then connect the components after understanding their individual responsibilities.

How I Used AI

I used AI to help me break down the complex FastAPI code into smaller concepts.

I asked AI to:

explain the Repository pattern;
explain Generic[T];
explain FastAPI dependency injection;
explain role-based access control;
trace the execution flow of the admin endpoint;
explain asynccontextmanager and lifespan events;
explain middleware;
simplify JWT authentication;
guide me through adding a logging system.

I used the explanations to build my own understanding rather than simply copying a complete implementation.

Final Reflection

This exercise showed me that understanding existing code is just as important as writing new code.

I learned that design patterns are useful because they give different parts of an application clear responsibilities.

The Repository pattern was useful for separating database operations, while dependency injection helped provide resources such as database sessions and authenticated users.

Tracing the execution flow also helped me understand where new functionality could be added without putting unrelated responsibilities into the same function.

My main takeaway is that when I encounter complex code, I should break it down into smaller components, understand the purpose of each component and then follow how they interact with one another.
