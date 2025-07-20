# Normal Functions

In Hyac, functions are primarily divided into two types: **Endpoint Functions** and **Common Functions**.

-   **Endpoint Function**: A function that can be accessed directly via an API, serving as the entry point for business logic.
-   **Common Function**: Similar to a reusable code library, it cannot be called directly via an API but can be referenced by endpoint functions.

## 1. Writing a Basic Endpoint Function

An endpoint function is your window to the outside world. Each endpoint function must define an asynchronous function named `handler`.

The parameters for the `handler` function are flexible, but `context` is a required first parameter.

```python
async def handler(context, request):
    """
    A basic endpoint function that returns fixed JSON data.
    """
    return {"code": 0, "msg":"success", "data":[1,2,3]}
```

-   `context`: **(Required)** The context object, which contains environment information such as database connections and common function modules.
-   `request`: **(Optional)** The FastAPI request object, which contains detailed information about the request, such as headers and query parameters. You can omit this parameter if your function does not need to access the raw request information.

You can also define additional parameters for the `handler` function, which will be automatically injected from the request's query parameters or body, relying on FastAPI's dependency injection feature.

```python
# A version that omits the request parameter
async def handler(context, name: str = "World"):
    """
    An endpoint function that accepts a parameter.
    If the request includes a 'name' parameter (e.g., ?name=Hyac), it will be automatically injected.
    """
    return {"hello": name}
```

## 2. Reusing Code with Common Functions

When your business logic becomes complex, putting all the code in a single `handler` function can become difficult to maintain. In such cases, you can extract the reusable parts into **common functions**.

A common function module can contain any number of functions and classes.

### Step 1: Create a Common Function

First, create a common function module. For example, let's create a module named `math_utils` for mathematical operations.

```python
# Common Function Module: math_utils
from loguru import logger

def add(a, b):
    """A simple addition function."""
    logger.info(f"Executing add({a}, {b})")
    return a + b

class AdvancedCalculator:
    """A calculator class that supports setting precision."""
    def __init__(self, precision: int = 2):
        self.precision = precision
        logger.info(f"AdvancedCalculator initialized with precision {self.precision}")

    def multiply(self, a, b):
        return a * b
```

### Step 2: Call it from an Endpoint Function

Then, in your endpoint function, you can call this common function module through the `context.common` object.

```python
# Endpoint Function
from loguru import logger

async def handler(context, x: int = 10, y: int = 3):
    """
    An example of an endpoint that calls a common function.
    This example assumes a common function named 'math_utils' exists.
    """
    
    # Call the common function via context.common.math_utils
    simple_sum = context.common.math_utils.add(x, y)
    logger.info(f"Called 'math_utils.add', result: {simple_sum}")

    # Use a class from the common module
    Calculator = context.common.math_utils.AdvancedCalculator
    calc_instance = Calculator(precision=4)
    product = calc_instance.multiply(x, y)
    
    return {
        "code": 0,
        "msg": "Calculation successful",
        "data": {
            "simple_addition": simple_sum,
            "product": product
        }
    }
```

This approach allows you to break down complex business logic into multiple, maintainable common modules, keeping your endpoint functions clean and concise.
