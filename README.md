# Technical Writer Tasks API Developers Guide

This guide provides developers with instructions on how to interact with the Technical Writer Tasks API using YAML code defined in the OpenAPI Specification version 3.0.0.
This developer's guide provides a comprehensive overview of the Technical Writer Tasks API, including authentication, available endpoints, parameters, responses, and example usage. Developers can use this guide to effectively integrate with and utilize the API for managing technical writing tasks.

The Technical Writer Tasks API allows developers to manage tasks related to technical writing. This includes retrieving all tasks, filtering tasks based on various parameters, and adding new tasks.

Intro
Purpose

## API Information
- Title: Technical Writer Tasks API
- Description: API for managing technical writer tasks.
- Version: 1.0.0
- Base URL: https://api.techwriter.xyz

## Authentication
The API uses API key authentication. Include the API key in the request header with the name Authorization.

## Endpoints

### Get existing tasks
To retrieve a list of all technical writing tasks, use:

**GET /tasks**

The following table details the parameters for this endpoint.
| Parameter    | Required?  |  Description |
| -----------  | ---------- | ----------- |
| status       | Optional   | Filter tasks by status.    |
| component    | Optional   | Filter tasks by component. |
| updatedAfter | Optional   | Filter tasks updated after a specific date. |

Responses:
- 200: An array of tasks.
- 401: Unauthorized, API key missing or invalid.

### Create a new task
To create a new technical writing task, use:

**POST /task**

Request body: TaskInput model.

Responses:
- 200: Task added successfully.
- 401: Unauthorized, API key missing or invalid.

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



In addtion, there is a security schema:
- ApiKeyAuth: API key authentication.



Example Usage
Here's an example of how to interact with the API:

```yaml
openapi: 3.0.0
info:
  title: Technical Writer Tasks API
  description: API for managing technical writer tasks.
  version: 1.0.0
servers:
  - url: https://api.techwriter.xyz
components:
  - Security Scheme and Schemas defined here...
paths:
  /tasks:
    get:
      # GET /tasks operation details...
    post:
      # POST /task operation details...
```
