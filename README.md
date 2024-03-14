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
**GET /tasks:**

Retrieves all technical writer tasks.
Parameters:
status: Filter tasks by status (optional).
component: Filter tasks by component (optional).
updatedAfter: Filter tasks updated after a specific date (optional).
Responses:
200: An array of tasks.
401: Unauthorized, API key missing or invalid.

### Create a new task
**POST /task:**

Adds a new technical writer task.
Request body: TaskInput model.
Responses:
200: Task added successfully.
401: Unauthorized, API key missing or invalid.

### Schemas
The endpoints use the following schemas:
- Task: Represents a technical writer task.
- TaskInput: Input model for creating a new task.
- ConnectedTask: Represents a connected task.
- StatusEnum: Enumeration of task statuses (OPEN, IN_PROGRESS, COMPLETED).
- ComponentEnum: Enumeration of task components (API_DOCS, HELP_CENTER, SDK_DOCS, OAS_FILE).

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
