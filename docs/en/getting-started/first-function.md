# Your First Function

This guide will walk you through the process of creating and deploying your first function on Hyac.

## 1. Create an Application

Before you can create a function, you need to create an application to house it.

1.  Navigate to the "Application Management" page.
2.  Click the "New Application" button.
3.  Fill in the required information and click "Create".

## 2. Create a Function

Now that you have an application, you can create a function.

1.  Navigate to the "Function Management" page.
2.  Click the "New Function" button.
3.  Select the application you just created.
4.  Fill in the required information, including the function code.
5.  Click "Create".

## 3. Invoke the Function

Once the function is created, you can invoke it using the provided API endpoint. You can find the endpoint on the function's details page.

You can use a tool like `curl` or Postman to send a request to the endpoint.

```bash
curl -X POST https://your-domain.com/api/v1/functions/invoke/your-function-name
