# Exercise 1: Getting Started with FastAPI

## Objective

The purpose of this exercise was to use AI to get started with FastAPI and understand the basic concepts involved in building an API with a new Python framework.

I used AI as a learning partner to help me understand FastAPI, its terminology, how its basic structure works, and how the different parts of a simple API fit together.

---

## Part 1: Understanding FastAPI Fundamentals

### What is FastAPI?

I learned that FastAPI is a Python web framework that is mainly used to build APIs. What stood out to me was that it makes strong use of Python type hints and Pydantic for validation.

Compared with Flask and Django, FastAPI is focused strongly on building APIs. Flask is lightweight and flexible, while Django provides a much larger set of features for building complete web applications. FastAPI provides features that are particularly useful when creating modern APIs, including automatic documentation and request validation.

### Core concepts I learned

Some of the important FastAPI concepts I learned were:

- `FastAPI()` creates the application instance.
- Routes define the URLs that an API responds to.
- HTTP methods such as GET and POST are used to describe what an endpoint does.
- Path parameters allow values to be included directly in a URL.
- Query parameters are values that can be supplied through the URL.
- Pydantic models are used to validate and structure data.
- `Depends()` is used for dependency injection.
- FastAPI automatically generates interactive API documentation.
- Type hints are used to describe the expected data types.

### FastAPI terminology

| Term | My understanding |
|---|---|
| FastAPI | A Python framework for building APIs |
| API | A way for different software applications to communicate |
| Endpoint | A specific URL where an API provides a function |
| Route | The path and HTTP method used to access an endpoint |
| Path parameter | A value included as part of the URL path |
| Query parameter | A value supplied after `?` in a URL |
| Pydantic | A library used for data validation and modelling |
| Dependency Injection | A way for FastAPI to provide a function with something it needs |
| `Depends()` | FastAPI's mechanism for declaring dependencies |
| Uvicorn | A server that can be used to run the FastAPI application |
| Swagger UI | The interactive API documentation available through `/docs` |

---

## Part 2: Creating My First API

### Basic FastAPI structure

I learned that a basic FastAPI application can be created by importing `FastAPI`, creating an application instance and then defining routes using decorators.

For example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World from FastAPI!"}
```

The @app.get("/") part tells FastAPI that the function underneath it should handle GET requests to the root URL.

The function then returns a Python dictionary, which FastAPI can return as JSON.

### Path parameters

I also learned how FastAPI handles path parameters. A path parameter is a value that forms part of the URL.

For example:

```python
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```
### Query parameters

In this example, {item_id} is part of the URL and FastAPI passes its value to the function.

The : int type hint tells FastAPI that the value should be an integer. This also allows FastAPI to validate the input.

I learned that query parameters can be used when information is supplied after the ? in a URL.

For example:

```python
@app.get("/search/")
async def search_items(q: str = None, skip: int = 0, limit: int = 10):
    return {
        "query": q,
        "skip": skip,
        "limit": limit
    }
```

A request such as:

`/search?q=test&skip=0&limit=10`

can provide values for the query parameters.

This helped me understand the difference between information contained directly in a URL path and information supplied as query parameters.

Automatic documentation

One of the features I found useful was FastAPI's automatic documentation.

The /docs endpoint provides an interactive interface where the available API endpoints can be viewed and tested.

I found this useful because it means I do not have to create all of the API documentation manually before being able to see how the API works.

Part 3: Enhancing the API
Pydantic models

I learned that Pydantic models can be used to define the structure and validation rules for data received by an API.

For example:

```python
from pydantic import BaseModel, Field


class ItemCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    price: float = Field(..., gt=0)
```

This means that an item must have a name and a price, and the price must be greater than zero.

This helped me understand that validation can be built into the model instead of manually checking every value inside the route.

Error handling

I also learned that FastAPI provides HTTPException for returning appropriate HTTP errors.

For example:

```python
from fastapi import HTTPException


if item_id not in fake_items_db:
    raise HTTPException(
        status_code=404,
        detail="Item not found"
    )
```

This allows the API to communicate clearly when something goes wrong.

Organising an application

As the application becomes larger, keeping everything in one Python file can become difficult to manage.

I learned that FastAPI applications can be divided into different modules for:

routes
models
utilities
exception handling
application configuration

This makes the application easier to maintain because different responsibilities can be separated.

Part 4: Exercise Challenge
To-do list API

The final challenge was to think about how I could use what I learned to create a simple to-do list API.

The application needs to support:

Creating a new to-do item.
Adding a title, description and due date.
Listing to-do items.
Filtering items by completed or pending status.
Marking an item as completed.
Deleting an item.
Validating the data using Pydantic.
Handling errors appropriately.
Providing automatic API documentation.
My approach

I would start by creating Pydantic models for the to-do data. I would then create separate endpoints for creating, listing, updating and deleting items.

I would also use validation to make sure that the data supplied to the API follows the expected structure.

For the list endpoint, I would use a status parameter to allow the user to retrieve either completed or pending tasks.

For operations involving a specific to-do item, I would use a path parameter containing the item's ID.

What I learned from this exercise

This exercise helped me understand the basic structure of a FastAPI application instead of only seeing FastAPI as another Python library.

The biggest thing I learned was how the different pieces connect. A route receives a request, parameters can be validated automatically, the function performs the required operation, and FastAPI converts the result into a response.

I also learned that type hints are more useful than I initially thought. In FastAPI, they help describe what kind of data an endpoint expects and allow the framework to perform validation.

Another feature that stood out to me was the automatic documentation because I could see how the API structure can be exposed through /docs.

How I used AI

I used AI as a learning assistant rather than only asking it to generate code.

I used prompts to:

understand what FastAPI is;
compare FastAPI with other Python frameworks;
explain FastAPI terminology;
understand routes, path parameters and query parameters;
understand Pydantic validation;
understand error handling;
understand how a FastAPI project can be organised;
break down code that I did not understand.

When I received code from AI, I focused on understanding what each part was doing instead of treating the generated code as something I should automatically accept.

Reflection

At the beginning, FastAPI looked like a lot of new terminology and syntax. Breaking the framework into smaller concepts made it easier for me to understand.

I found the idea of dependency injection more difficult than basic routing because it was a new way of thinking about how functions receive the things they need. However, learning the concepts separately made the larger examples easier to follow.

I also realised that using AI effectively requires me to ask specific questions. A vague question can give me a broad answer that does not necessarily solve the thing I am struggling with. Giving AI context and asking about one concept at a time made the learning process more useful.

This exercise also reinforced the idea that I should not depend on AI to do all the thinking for me. I need to understand the code and be able to explain why it works.

