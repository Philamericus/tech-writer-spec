# Technical Writer Tasks Developers Guide

This developer's guide provides a comprehensive overview of the **Technical Writer Tasks API**, including authentication, available endpoints, parameters, responses, and example usage. Developers can use this guide to effectively integrate with and utilize the API for managing technical writing tasks.

The Technical Writer Tasks API allows developers to manage tasks related to technical writing. This includes retrieving all tasks, filtering tasks based on various parameters, and adding new tasks.

## API Information
The information in the following sections is relevant for the Technical Writer Tasks API version 1.0.0.

The API uses the base URL: `https://api.techwriter.xyz`

## Authentication
The API uses API key authentication. 

To create an API key, call the **POST /auth/token** endpoint and include your username and password in the request body. A successful response contains the API key. 

Include the API key in the request header with the name *Authorization* to authenticate and access protected endpoints. Note that for the *Authorization* value you must use the format of `Bearer YOUR_API_KEY`.

See the endpoint description for [/auth/token](#generate-an-authorization-key) below for more information.

## Endpoint Descriptions

Endpoints are provided to:
- POST /auth/token - [Generate an Authorization Key](#generate-an-authorization-key)
- GET /tasks - [Get Existing Tasks](#get-existing-tasks)
- POST /tasks - [Create a New Task](#create-a-new-task)

<a id="generate-an-authorization-key"></a>
### Generate an Authorization Key
**POST /auth/token**

Generates an authorization key based on a username and password.

The request body is a JSON object with properties: username, password.

An example request body is:
```
{
  "username": "string",
  "password": "string"
}
```

Responses:
- 200:
  - Authorization key generated successfully.
  - Content: JSON object with token property.
- 401:
  - Unauthorized. Invalid username or password.
  
**Note:** The generated token should be included in subsequent requests as an API key with the format `Bearer YOUR_API_KEY`.

#### Python Code Example
```python
import requests

# Define the endpoint URL
url = 'https://api.techwriter.xyz/auth/token'

# Define the request body with username and password
data = {
    'username': 'your_username',
    'password': 'your_password'
}

# Make the POST request
response = requests.post(url, json=data)

# Check if the request was successful
if response.status_code == 200:
    # Extract the authorization token from the response
    token = response.json()['token']
    print("Authorization token:", token)
else:
    print("Failed to generate authorization token:", response.status_code)
```

#### JavaScript Code Example
```
const fetch = require('node-fetch');

// Define the endpoint URL
const url = 'https://api.techwriter.xyz/auth/token';

// Define the request body with username and password
const data = {
    username: 'your_username',
    password: 'your_password'
};

// Make the POST request
fetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
})
.then(response => {
    // Check if the request was successful
    if (response.status === 200) {
        // Extract the authorization token from the response
        return response.json();
    } else {
        throw new Error('Failed to generate authorization token');
    }
})
.then(data => {
    const token = data.token;
    console.log("Authorization token:", token);
})
.catch(error => console.error('Error:', error));
```

<a id="get-existing-tasks"></a>
### Get Existing Tasks
**GET /tasks**

Retrieves all technical writer tasks with optional filtering by status, component, or updated date.

An authorization header with an API key is required with each request.

The following table details the parameters for this endpoint.
| Name         | Type   | Required  | Description |
| -----------  | ------ | --------- | ----------- |
| status       | string | Optional  | Filter tasks by status. See the `StatusEnum` schema.   |
| component    | string | Optional  | Filter tasks by component. See the `ComponentEnum` schema. |
| updatedAfter | string | Optional  | Filter tasks updated after a specific date. |

- Responses:
  - 200:
    - An array of tasks. Refer to the `Tasks` schema for the format of the response.
  - 401:
    - Unauthorized, API key missing or invalid.

And example of a successful response is:
```json
[
  {
    "task_id": "123",
    "title": "Write API Documentation",
    "description": "Create documentation for the new API endpoints.",
    "status": "IN_PROGRESS",
    "component": "API_DOCS",
    "connected_tasks": []
  },
  {
    "task_id": "456",
    "title": "Update SDK Documentation",
    "description": "Update the SDK documentation with the latest changes.",
    "status": "OPEN",
    "component": "SDK_DOCS",
    "connected_tasks": []
  }
]
```

<a id="create-a-new-task"></a>
### Create a New Task
**POST /tasks**

This endpoint creates a new technical writing task.

An authorization header with an API key is required with each request.

The request body uses the `TaskInput` schema. 

An example request body is:
```
{
  "title": "Write Release Notes",
  "description": "Draft release notes for version 2.0 of the product.",
  "status": "OPEN",
  "component": "HELP_CENTER",
  "connected_tasks": []
}
```

Responses:
- 200:
  - Task added successfully.
- 401:
  - Unauthorized, API key missing or invalid.

The following sections provide sample code in Python and JavaScript.

**Note:** In these examples, replace `YOUR_API_KEY` with your actual API key when making requests.

#### Python Code Example
```python
import requests

url = "https://api.techwriter.xyz/tasks"
payload = {
    "title": "Write Release Notes",
    "description": "Draft release notes for version 2.0 of the product.",
    "status": "OPEN",
    "component": "HELP_CENTER",
    "connected_tasks": []
}
headers = {
    "Authorization": "Bearer YOUR_API_KEY"
}

response = requests.post(url, json=payload, headers=headers)
print(response.status_code)
print(response.json())
```

#### JavaScript Code Example
```javascript
const fetch = require('node-fetch');

const url = "https://api.techwriter.xyz/tasks";
const payload = {
    title: "Write Release Notes",
    description: "Draft release notes for version 2.0 of the product.",
    status: "OPEN",
    component: "HELP_CENTER",
    connected_tasks: []
};
const headers = {
    "Authorization": "Bearer YOUR_API_KEY",
    "Content-Type": "application/json"
};

fetch(url, {
    method: 'POST',
    headers: headers,
    body: JSON.stringify(payload)
})
.then(response => {
    console.log(response.status);
    return response.json();
})
.then(data => console.log(data))
.catch(error => console.error('Error:', error));
```

### Schemas
The API has the following schemas along with a description of the purpose and structure of each schema:

1. **Task**:
   - Description: Represents a task in the task management system.
   - Properties:
     - `task_id`: Unique identifier for the task.
     - `title`: Title or name of the task.
     - `description`: Description of the task.
     - `status`: Status of the task, referencing the `StatusEnum` schema.
     - `component`: Component associated with the task, referencing the `ComponentEnum` schema.
     - `connected_tasks`: Array of connected tasks, referencing the `ConnectedTask` schema.

2. **TaskInput**:
   - Description: Input model for creating a new task in the task management system.
   - Properties:
     - `title`: Title or name of the task.
     - `description`: Description of the task.
     - `status`: Status of the task, referencing the `StatusEnum` schema.
     - `component`: Component associated with the task, referencing the `ComponentEnum` schema.
     - `connected_tasks`: Array of connected tasks, referencing the `ConnectedTask` schema.

3. **ConnectedTask**:
   - Description: Represents a task connected to another task in the task management system.
   - Properties:
     - `task_id`: Unique identifier for the connected task.
     - `title`: Title or name of the connected task.
     - `description`: Description of the connected task.
     - `status`: Status of the connected task, referencing the `StatusEnum` schema.

4. **StatusEnum**:
   - Description: Enumeration of possible task statuses.
   - Values: OPEN, IN_PROGRESS, COMPLETED.

5. **ComponentEnum**:
   - Description: Enumeration of possible components associated with tasks.
   - Values: API_DOCS, HELP_CENTER, SDK_DOCS, OAS_FILE.


# Best Practices
The following are best practices for developing with the Technical Writing Tasks API.
1. **Error codes**. When calling an endpoint, verify that the return code is checked for each request. Any return code other than 200 indicates an error.
2. **API key security**. The username and password and corresponing authorization key should be kept secure. When possible, do not hardcode these values into your code. Verify that the username, password, and authorization key are not publically available.
3. **Handling rate limits**. If you anticipate high usage of these endpoints, implement a handling rate limit corresponding to your server-side resources.
