# Exercise 3: Documentation Navigation for FastAPI

## Objective

The purpose of this exercise was to practise using technical documentation to learn FastAPI and to connect information from the documentation to practical code examples.

I focused on understanding how to find useful information in documentation instead of relying only on generated answers.

---

## Part 1: Documentation Summarisation

### Suggested learning order

I learned that a useful way to approach the FastAPI documentation is to start with the basic concepts before moving into more advanced features.

My suggested learning order is:

1. First Steps
2. Path Parameters
3. Query Parameters
4. Request Body
5. Response Model
6. Dependencies
7. Security
8. Middleware
9. Background Tasks
10. Advanced topics

This order makes sense to me because it starts with the basic structure of an API and gradually introduces more advanced features.

### Important documentation sections

The five sections I considered most important when building a REST API quickly are:

- First Steps
- Path Parameters
- Query Parameters
- Request Body
- Dependencies

These sections cover many of the concepts needed to create and structure basic API endpoints.

---

## Part 2: Documentation Deep Dive

### Dependency Injection

One of the concepts I explored was dependency injection.

FastAPI uses the `Depends()` function to declare dependencies.

For example:

```python
from fastapi import Depends, FastAPI

app = FastAPI()

def get_user():
    return {"username": "demo"}

@app.get("/profile")
async def profile(user: dict = Depends(get_user)):
    return user
```

In this example, FastAPI calls `get_user()` and provides the result to the `profile()` function.

I learned that dependencies can be useful for shared functionality such as authentication, database connections and permissions.

### Why `Depends()` is useful

Using dependencies prevents the same logic from being repeated in multiple routes.

For example, authentication logic can be placed in one dependency and then reused by several protected endpoints.

This makes the application easier to maintain.

---

## Part 3: Concept to Code Translation

### Pydantic models

I learned that Pydantic models allow me to describe the structure of data expected by an API.

For example:

```python
from pydantic import BaseModel

class UserCreate(BaseModel):
    username: str
    email: str
    password: str
```

The model communicates what information is expected when creating a user.

### Path operation parameters

FastAPI can automatically handle different types of parameters.

For example:

```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/{item_id}")
async def read_item(
    item_id: int,
    q: str | None = Query(None)
):
    return {
        "item_id": item_id,
        "query": q
    }
```

The `item_id` is a path parameter, while `q` is a query parameter.

The type hints also allow FastAPI to validate the supplied values.

### Background tasks

I also learned that FastAPI provides background tasks for operations that can happen after a response is returned.

A simple example is:

```python
from fastapi import BackgroundTasks, FastAPI

app = FastAPI()

def send_message(message: str):
    print(message)

@app.post("/send")
async def send(background_tasks: BackgroundTasks):
    background_tasks.add_task(send_message, "Message sent")
    return {"message": "Request received"}
```

This can be useful for tasks such as sending notifications or performing other operations that do not need to delay the response.

### Exception handling

FastAPI provides `HTTPException` for returning errors to clients.

For example:

```python
from fastapi import HTTPException

if item_id < 1:
    raise HTTPException(
        status_code=400,
        detail="Item ID must be positive"
    )
```

This allows the API to return a clear status code and explanation when something goes wrong.

---

## Part 4: Comprehensive Documentation Challenge

### Blog API

The final challenge was to think about how the FastAPI documentation could guide the development of a small blog API.

The application would need to support:

- User registration
- User authentication
- Creating blog posts
- Reading blog posts
- Updating blog posts
- Deleting blog posts
- Adding comments
- Searching for posts

### Documentation areas I would use

For user registration and authentication, I would use the security and request body sections.

For blog posts, I would use path operations, request bodies, response models and status codes.

For comments, I would use path parameters and request body models.

For searching, I would use query parameters.

For authentication and protecting endpoints, I would use dependencies and security features.

### How the documentation influenced my approach

The documentation helped me understand that I should not try to build the entire application at once.

I would divide the application into smaller features and use the relevant documentation for each feature.

For example, I would first create the basic routes, then add Pydantic models, then authentication and finally additional functionality such as comments and search.

This approach would make it easier to test each part before adding the next one.

---

## Reflection

This exercise taught me that technical documentation is not something I should only look at when I am completely stuck.

I can use documentation as a learning tool from the beginning of a project.

I also learned that it is easier to work with large documentation sites when I have a specific question or feature that I am trying to understand.

The dependency injection section was useful because it showed me how FastAPI can organise reusable functionality.

I also found the connection between documentation and code useful. Instead of only reading definitions, I could look at a concept and then see how it is actually used in an API.

### How I used AI

I used AI to help me navigate the documentation and translate technical explanations into language that I could understand.

I asked focused questions about dependency injection, path operation decorators, background tasks and exception handling.

I also used AI to help connect documentation concepts to practical examples.

I did not treat the AI responses as automatically correct. I compared the explanations with the concepts in the documentation and focused on understanding why the examples worked.

### Final reflection

This exercise improved my confidence in using documentation when learning a new framework.

I learned that I do not need to read every page from beginning to end. I can identify what I need, find the relevant section, understand the example and then apply the concept.

This is a useful skill because software developers regularly need to learn unfamiliar libraries, frameworks and APIs.
