# Exercise 2: Contextual Learning with FastAPI

## Objective

The purpose of this exercise was to learn FastAPI by comparing it with concepts and frameworks I already understand. I used AI to help me connect new FastAPI concepts to familiar programming ideas instead of trying to learn everything from the beginning.

---

## Part 1: Framework Comparison

### FastAPI and Flask

I learned that FastAPI and Flask are both Python web frameworks that can be used to build APIs.

The concepts that are similar include routes, HTTP methods, request handling and returning responses. Both frameworks allow me to define routes such as GET and POST endpoints.

One important difference is that FastAPI makes extensive use of Python type hints and automatic validation. FastAPI also provides automatic API documentation, while Flask gives me more freedom and requires me to choose additional tools for many features.

This helped me understand that although the frameworks can be used for similar purposes, they have different approaches.

### FastAPI and Django

I learned that FastAPI's dependency injection system is not exactly the same as Django's middleware.

Middleware generally works at the request and response level and can process requests before they reach a view and responses before they are returned to the client.

FastAPI's dependency injection system is more focused on providing a route with the things it needs. For example, a route can use `Depends()` to receive a database session or the currently authenticated user.

This helped me understand that middleware and dependencies can both be used to share functionality, but they solve different problems.

### Flask Blueprints and FastAPI Routers

I learned that FastAPI's `APIRouter` can be used in a similar way to Flask Blueprints.

Both approaches allow an application to separate routes into different sections instead of putting everything into one large file.

For example, a larger FastAPI application could have separate routers for users, products and authentication.

### Request validation

I learned that FastAPI can automatically validate request data using Python type hints and Pydantic models.

This is useful because I do not have to manually check every value inside my route functions.

For example:

```python
from pydantic import BaseModel

class User(BaseModel):
    username: str
    age: int
```
FastAPI can use this model to check that the incoming data has the expected structure and data types.

## Part 2: Understanding FastAPI Design Choices
### Why FastAPI uses Pydantic

I learned that Pydantic provides a convenient way to define and validate data using Python models.

Instead of creating a separate validation system, FastAPI can use Pydantic models to describe the data an API expects.

This makes the code easier to understand because the structure and validation rules are defined together.

### Automatic API documentation

One feature that stood out to me was FastAPI's automatic documentation.

FastAPI can generate interactive documentation from the application's routes, parameters, type hints and models.

The documentation is available through endpoints such as /docs.

I found this useful because it makes it easier to understand and test an API while developing it.

Why FastAPI uses type hints

I learned that type hints are important in FastAPI because they help describe the data an endpoint expects.

For example:

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}

The int type hint tells FastAPI that item_id should be an integer.

Type hints therefore help with validation, documentation and making the code easier to understand.

Async support

I learned that FastAPI provides strong support for asynchronous programming using async and await.

This is useful for applications that need to handle many operations involving waiting, such as database queries or external API requests.

I also learned that using async does not automatically make every operation faster. It is most useful when the application is performing operations that can benefit from asynchronous execution.

Part 3: Applied Contextual Learning
Authentication with JWT

I used the example of JWT authentication to understand how authentication can be implemented in FastAPI.

The basic process I learned is:

A user provides a username and password.
The application checks whether the credentials are correct.
If they are correct, the application creates a JWT access token.
The client uses the token when making requests to protected endpoints.
FastAPI can use a dependency to retrieve and validate the token.
If the token is valid, the current user can access the protected endpoint.

A simplified example is:

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer


app = FastAPI()


oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


@app.get("/users/me")
async def read_users_me(token: str = Depends(oauth2_scheme)):
    return {"token": token}

The important part for me was understanding what Depends() is doing. FastAPI provides the value returned by the dependency to the route function.

In a real application, the token would need to be properly decoded and validated, and passwords would need to be securely hashed.

Part 4: Mental Model Translation

I created the following translation table to connect concepts I already understand with FastAPI concepts.

Familiar web development concept	FastAPI concept	My understanding
Route	Path operation	Defines how the API responds to a URL and HTTP method
Flask Blueprint	APIRouter	Helps organise related routes
Form/data validation	Pydantic models	Defines and validates incoming data
Shared functionality	Dependencies	Provides functions or resources needed by routes
Middleware	Middleware	Processes requests and responses globally
View/controller logic	Path operation function	Contains the logic for an endpoint
API documentation	/docs	Automatically generated interactive documentation
Type declarations	Python type hints	Describe the expected data and support validation

This translation table helped me realise that learning a new framework does not mean everything is completely new. Many of the underlying concepts are familiar, but the framework uses different tools and patterns to implement them.

What I Learned

The most useful part of this exercise was learning FastAPI through concepts that I already understood.

I found it easier to understand APIRouter when I compared it with Flask Blueprints. I also understood dependency injection better after seeing how it could provide things such as database sessions or authenticated users to a route.

I learned that FastAPI's design makes heavy use of Python's existing features, especially type hints. These type hints are not only useful for making code readable but also help FastAPI with validation and documentation.

I also learned that FastAPI is designed to make building APIs more straightforward by combining routing, validation, dependency injection and automatic documentation.

Reflection

Before this exercise, I thought learning a new framework meant learning a completely new way of programming. Comparing FastAPI with concepts I already knew showed me that many of the underlying ideas are actually similar.

I found dependency injection to be one of the more difficult concepts because it was less familiar to me. However, seeing it used with Depends() helped me understand that it is mainly a way of providing a function with the resources or information it needs.

I also realised that comparing new concepts with things I already understand is a useful learning strategy. Instead of memorising FastAPI terminology, I can build connections between the new framework and my existing knowledge.

How I Used AI

I used AI as a learning partner throughout this exercise.

I asked AI to:

compare FastAPI with Flask and Django;
explain the differences between middleware and dependency injection;
explain the purpose of APIRouter;
explain Pydantic validation;
explain why FastAPI uses type hints;
explain JWT authentication;
translate FastAPI concepts into concepts I already understood;
explain code examples when I was unsure about their purpose.

I did not want AI to simply provide a finished application for me. I focused on asking questions about why FastAPI uses particular patterns so that I could understand the reasoning behind the code.

Final Reflection

This exercise helped me see that AI can be useful when learning a new framework if I use it to connect new information to knowledge I already have.

I also learned that asking specific questions gives me more useful answers than asking AI to explain an entire framework at once.

My main takeaway is that I should approach a new framework by identifying familiar concepts first, finding their equivalents, and then learning the differences. This makes the learning process less overwhelming and helps me build a stronger mental model of the framework.
