# Exercise 2: Contextual Learning with FastAPI

## Objective

The purpose of this exercise was to learn FastAPI by comparing it with concepts from other Python web frameworks and by using my existing programming knowledge as a starting point.

I used AI to help me understand the similarities and differences between FastAPI and frameworks such as Flask and Django.

---

## Part 1: Framework Comparison

### FastAPI and Flask

I learned that FastAPI and Flask are both Python frameworks that can be used to build web applications and APIs.

Flask is lightweight and gives the developer a lot of flexibility in how the application is structured. FastAPI is also relatively lightweight, but it provides features specifically useful for building APIs, such as automatic validation, type hints and automatic API documentation.

One similarity is that both frameworks use route decorators to connect URLs to Python functions.

In FastAPI, for example:

```python
@app.get("/")
async def root():
    return {"message": "Hello World"}
```

The decorator tells the framework which HTTP method and URL should be handled by the function.

### FastAPI and Django

Django is a larger web framework that provides many features for building complete web applications, including models, authentication and an administrative interface.

FastAPI is more focused on APIs and allows developers to choose the additional libraries and tools they need.

I learned that FastAPI does not try to provide everything in one framework. Instead, it provides a strong foundation for building APIs.

### Translation table

| Concept I know | FastAPI equivalent | My understanding |
|---|---|---|
| Route | Path operation | Connects a URL and HTTP method to a function |
| Flask route | FastAPI path operation | Both connect URLs to functions |
| Request validation | Pydantic model | Defines and validates incoming data |
| Dependency | `Depends()` | Provides a function with something it needs |
| Blueprint | `APIRouter` | Helps organise routes into separate modules |
| View function | Path operation function | Contains the logic for an endpoint |
| Middleware | Middleware | Runs around requests and responses |
| API documentation | `/docs` | Provides interactive API documentation |

---

## Part 2: Understanding Design Choices

### Why FastAPI uses Pydantic

I learned that Pydantic allows FastAPI to validate and structure data using Python type hints.

Instead of manually checking every field inside a route, I can define a model that describes what the data should look like.

For example:

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    age: int
```

This makes the expected structure of the data clear and allows invalid data to be detected automatically.

### Why FastAPI uses type hints

Type hints make the expected data types clearer.

For example:

```python
def read_item(item_id: int):
    return {"item_id": item_id}
```

The `int` tells FastAPI that `item_id` should be an integer.

I learned that these type hints are useful for both validation and documentation.

### Automatic documentation

FastAPI automatically generates interactive documentation from the application structure.

The `/docs` endpoint provides Swagger UI, where endpoints can be viewed and tested.

I found this useful because the API documentation is connected directly to the code instead of having to be created separately.

### Async support

I learned that FastAPI supports asynchronous programming using Python's `async` and `await` syntax.

This can be useful for applications that spend time waiting for operations such as database queries or network requests.

I also learned that using `async` does not automatically make every operation faster. It is most useful when working with operations that can benefit from asynchronous execution.

---

## Part 3: Applied Contextual Learning

### JWT authentication

The implementation challenge was to understand how authentication using JWT tokens can be implemented in FastAPI.

The basic process I learned was:

1. A user provides their username and password.
2. The application verifies the credentials.
3. If the credentials are correct, the application creates a JWT token.
4. The client sends the token when accessing protected endpoints.
5. FastAPI uses a dependency to retrieve and validate the token.
6. The application identifies the current user.
7. Protected routes can then use the authenticated user.

A simplified example is:

```python
from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

@app.get("/users/me")
async def read_users_me(token: str = Depends(oauth2_scheme)):
    return {"token": token}
```

The important part for me was understanding the role of `Depends()`.

The route does not have to manually retrieve the authentication information. FastAPI provides the dependency and passes the result into the route function.

---

## Reflection

The contextual approach helped me understand FastAPI by connecting new concepts to things I already knew.

I found the comparison with Flask particularly useful because I could see that the basic idea of defining routes and connecting them to functions is familiar, even though FastAPI adds features such as validation and automatic documentation.

The concept of dependency injection was still one of the more difficult ideas for me. I understood it better when I looked at it as FastAPI providing a function with something it needs instead of the function creating or finding that dependency itself.

The JWT example also helped me understand how authentication can be separated from the actual endpoint logic.

I learned that comparing a new technology with something I already understand can make learning much easier.

### What I learned about using AI

I used AI to translate unfamiliar FastAPI concepts into concepts that were easier for me to understand.

Instead of only asking for complete solutions, I asked questions about why FastAPI was designed in a particular way and how its features compared with concepts from other frameworks.

This helped me focus on understanding the reasoning behind the code rather than only copying the syntax.

### Final reflection

This exercise showed me that I do not have to learn a completely new framework from zero. I can use concepts I already understand as a mental bridge.

I also learned that asking AI to compare concepts is useful when learning a new technology because it helps me identify what is familiar and what is genuinely new.
