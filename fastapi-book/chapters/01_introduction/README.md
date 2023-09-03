FastAPI is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints. It was created by Sebastián Ramírez and is built on top of Starlette for the web parts and Pydantic for the data parts.

**Setting up the development environment**

    python -m pip install --upgrade pip

    pip install fastapi

    pip install uvicorn[standard]

    pip install python-multipart

If you need to install the complete FastAPI platform, including all optional dependencies, the appropriate command is

    pip install fastapi[all].

Likewise, if you want to install and utilize the full-blown uvicorn server, you should run the

    pip install uvicorn

command. Also, install the bcrypt module for encryption-related tasks.

### REST API and HTTP Methods

REST (Representational State Transfer) APIs provide a way to interact with resources (which could be data objects, services, or entities) over HTTP.

When we say "REST APIs that are technically Python functions," it means that FastAPI allows you to define REST API endpoints using plain Python functions, decorated with FastAPI's path operation decorators to specify which HTTP method(s) they handle. This makes it incredibly straightforward to create a REST API: you define Python functions to handle different types of HTTP requests, decorate them appropriately, and FastAPI takes care of the rest.

## Designing and implementing REST APIs

The Representation State Transfer (REST) API makes up the rules, processes, and tools that allow interaction among microservices.
These are method services that are identified and executed through their endpoint URLs.

Nowadays, focusing on API methods before building a whole application is one of the most popular and effective microservices design strategies.

### Initializing and configuring FastAPI

A simple application can be created just by

    from fastapi import FastAPI
    app = FastAPI()

This initializes the FastAPI framework. The application needs to instantiate the core FastAPI class from the fastapi module and use app as the reference variable to the object. Then, this object is used later as a Python @app decorator, which provides our application with some features such as routes, middleware, exception handlers, and path operations.

Now, your application is ready to manage REST APIs that are technically Python functions. But to declare them as REST service methods, we need to decorate them with the appropriate HTTP request method provided by the path operation @app decorator.

To locally run our application, we need to execute the following command:

    uvicorn main:app --reload

After running the uvicorn server, we can check and validate whether all our URLs are valid and running by accessing the documentation URL, http://localhost:8000/docs. This path will show us a OpenAPI dashboard

## Managing user requests and server response

Clients can pass their request data to FastAPI endpoint URLs through path parameters, query parameters, or headers to pursue service transactions. There are standards and ways to use these parameters to obtain incoming requests.

Depending on the goal of the services, we use these parameters to influence and build the necessary responses the clients need. But before we discuss these various parameter types, let us explore first how we use type hinting in FastAPI’s local parameter declaration.

### What is Type Hinting?

Type hinting is a feature in Python (and consequently in FastAPI) that allows you to explicitly specify the type of a variable or function parameter. This is done to make the code more readable and maintainable. It's also used by tools like IDEs to provide better code completion and error checking. In FastAPI, type hinting plays a more critical role. It is used to:

- **Validate the data**: FastAPI uses the type hints to validate incoming requests, ensuring that the data conforms to the expected type.
- **Create API Documentation**: FastAPI can automatically generate interactive API documentation like Swagger UI or ReDoc, based on these type hints.
- **Serialize and Deserialize**: Type hints help FastAPI to automatically parse incoming JSON requests into Python data types and vice versa.


### Path parameters

FastAPI allows you to obtain request data from the endpoint URL of an API through a path parameter or path variable that makes the URL somewhat dynamic.

This parameter holds a value that becomes part of a URL indicated by curly braces ({}). After setting off these path parameters within the URL, FastAPI requires these parameters to be declared by applying type hints.

"Setting off" here means that you have defined which parts of your URL are dynamic and will change based on the input. For example, in /items/{item_id}, item_id will change based on the item you are querying.

FastAPI requires that you declare the type of these parameters explicitly. This is done by applying "type hints" in your Python function that handles the API endpoint. Type hints are a way to explicitly state the expected data type of a variable in Python.

For example, if you have an API endpoint defined like this in FastAPI:

    from fastapi import FastAPI

    app = FastAPI()

    @app.get("/items/{item_id}")
    def read_item(item_id: int):
        return {"item_id": item_id}

Here, `item_id: int` is applying a type hint of `int`. This tells FastAPI that the `item_id` in the URL must be an integer. FastAPI will automatically validate this for you. If someone tries to access `/items/some_string`, FastAPI will return an error because some_string is not an integer.


Multiple path parameters are acceptable if the leftmost variables are more likely to be filled with values than the rightmost variables. In other words, the importance of the leftmost path variables will make the process more relevant and correct than those on the right

    @app.get("/users/{user_id}/items/{item_id}")

#### Importance of Order: Leftmost vs Rightmost
Now, let's discuss the idea that "the leftmost variables are more likely to be filled with values than the rightmost variables."

In RESTful API design, it's common to order URL parameters based on their importance or likelihood of being specified. Parameters that are more "important" or that are more likely to be provided by the client should be placed on the left. Those that are optional or less likely to be provided should be on the right.


**Why Does This Matter?**
- **Readability**: The URL's structure often reflects the relationship between resources. Placing the most important or primary resources on the left makes the URL easier to read and understand.

- **Relevance**: If a client provides only some of the path parameters, having the most relevant ones on the left helps the API function in a meaningful way with partial information.

- **Routing**: Some web frameworks (including FastAPI) use the order of path parameters to resolve ambiguities in routing. The leftmost parameter often has priority in these situations.