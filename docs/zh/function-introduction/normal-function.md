# 常规函数

在 Hyac 中，函数主要分为两种类型：**端点函数 (Endpoint Function)** 和 **通用函数 (Common Function)**。

-   **端点函数**: 可以通过 API 直接访问的函数，是业务逻辑的入口。
-   **通用函数**: 类似于可复用的代码库，不能直接通过 API 调用，但可以被端点函数引用。

## 1. 编写一个基本的端点函数

端点函数是您与外部世界交互的窗口。每个端点函数都必须定义一个名为 `handler` 的异步函数。

`handler` 函数的参数非常灵活，但 `context` 是必需的第一个参数。

```python
async def handler(context, request):
    """
    一个基本的端点函数，返回固定的 JSON 数据。
    """
    return {"code": 0, "msg":"success", "data":[1,2,3]}
```

-   `context`: **(必需)** 上下文对象，包含了数据库连接、通用函数模块等环境信息。
-   `request`: **(可选)** FastAPI 的请求对象，包含了请求的详细信息，如请求头、查询参数等。如果您的函数不需要访问原始请求信息，可以省略此参数。

您还可以为 `handler` 函数定义额外的参数，这些参数会自动从请求的查询参数或请求体中获取，这依赖于 FastAPI 的依赖注入功能。

```python
# 省略 request 参数的写法
async def handler(context, name: str = "World"):
    """
    一个接收参数的端点函数。
    如果请求中包含 'name' 参数 (如 ?name=Hyac)，它将被自动注入。
    """
    return {"hello": name}
```

## 2. 使用通用函数进行代码复用

当您的业务逻辑变得复杂时，将所有代码都放在一个 `handler` 函数中会变得难以维护。这时，您可以将可复用的部分抽离到**通用函数**中。

通用函数模块可以包含任意数量的函数和类。

### 步骤 1: 创建通用函数

首先，创建一个通用函数模块。例如，我们创建一个名为 `math_utils` 的模块，用于执行数学运算。

```python
# 通用函数模块: math_utils
from loguru import logger

def add(a, b):
    """简单的加法函数"""
    logger.info(f"执行 add({a}, {b})")
    return a + b

class AdvancedCalculator:
    """一个支持设置精度的计算器类"""
    def __init__(self, precision: int = 2):
        self.precision = precision
        logger.info(f"AdvancedCalculator 初始化，精度为 {self.precision}")

    def multiply(self, a, b):
        return a * b
```

### 步骤 2: 在端点函数中调用

然后，在您的端点函数中，通过 `context.common` 对象来调用这个通用函数模块。

```python
# 端点函数
from loguru import logger

async def handler(context, x: int = 10, y: int = 3):
    """
    一个调用通用函数的端点示例。
    此示例假设存在一个名为 'math_utils' 的通用函数。
    """
    
    # 通过 context.common.math_utils 调用通用函数
    simple_sum = context.common.math_utils.add(x, y)
    logger.info(f"调用 'math_utils.add'，结果: {simple_sum}")

    # 使用通用模块中的类
    Calculator = context.common.math_utils.AdvancedCalculator
    calc_instance = Calculator(precision=4)
    product = calc_instance.multiply(x, y)
    
    return {
        "code": 0,
        "msg": "计算成功",
        "data": {
            "simple_addition": simple_sum,
            "product": product
        }
    }
```

通过这种方式，您可以将复杂的业务逻辑拆分成多个可维护的通用模块，使您的端点函数保持简洁和清晰。
