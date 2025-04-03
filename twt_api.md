# Technical Writer Task API

## Introduction

The Technical Writer Task API (TWT API) allows you to create and view all your current technical writing tasks. Along with adding titles and descriptions, the TWT API allows you to search your tasks by status, category, and even by latest update. You can also link related tasks together for easier tracking.

<br />

## Authentication Method

You can authenticate the TWT API using an API key (also known as an API token).

To obtain your API key, please sign up for a Fireblocks Developer Sandbox. We will provide you with an API key after approving your request.

If you are already a Fireblocks Developer, you can generate a new API Key in the developer portal.

<br />

## Endpoint descriptions

The TWT API currently has 2 endpoints, described below.

### GET /tasks

The GET /tasks request retrieves and lists all created technical writer tasks.

**URI**

[https://api.techwriter.xyz/tasks](https://api.techwriter.xyz/tasks)

**Header**

* X-API-KEY

**Query Parameters**

| Parameter      | Required? | Type         | Description                                           |
| :------------- | :-------- | :----------- | :---------------------------------------------------- |
| `status`       | N         | enum(string) | The position of the task in the workflow.             |
| `category`     | N         | enum(string) | A label used for organizing and tracking a task.      |
| `updatedAfter` | N         | string(date) | The date of the latest edit to the task. (YYYY-MM-DD) |

<br />

**Example request**

```python
import requests

url = "https://api.techwriter.xyz/tasks?status=IN_PROGRESS&category=API_DOCS"

headers = {
    "accept": "application/json",
    "X-API-KEY": "X-API-KEY"
}

response = requests.get(url, headers=headers)

print(response.text)

```

**Example response**

```json
[
  {
    "task_id": 17,
    "title": "Update POST /task endpoint request body",
    "description": "Complete the request body section for POST/task endpoint.",
    "status": "OPEN",
    "category": "API_DOCS",
    "last_updated": "2025-04-03",
    "connected_tasks": [
      {
        "task_id": 12,
        "title": "Validate Technical Writer Tasks API OAS",
        "description": "Correct errors in the existing TWT API OAS to validate the specification file.",
        "status": "OPEN"
      }
    ]
  }
]

```

### POST /task

The POST /task request creates a new technical writing task. You can also link a new task with existing tasks.

**URI**

[https://api.techwriter.xyz/task](https://api.techwriter.xyz/task)

**Header**

* X-API-KEY
* Content-Type

**Body Parameters**

| Parameter         | Required? | Type         | Description                                      |
| :---------------- | :-------- | :----------- | :----------------------------------------------- |
| `title`           | **Y**     | string       | The name of the task.                            |
| `description`     | **Y**     | string       | Detailed information about a task.               |
| `status`          | N         | enum(string) | The position of the task in the workflow.        |
| `category`        | N         | enum(string) | A label used for organizing and tracking a task. |
| `connected_tasks` | N         | object       | Tasks related to the identified task.            |

**Example request**

```python

import requests

url = "https://api.techwriter.xyz/task"

payload = {
    "status": "OPEN",
    "category": "API_DOCS",
    "connected_tasks": [12],
    "title": "Update POST /task endpoint request body",
    "description": "Complete the request body section for POST/task endpoint."
}
headers = {
    "accept": "application/json",
    "content-type": "application/json",
    "X-API-KEY": "X-API-KEY"
}

response = requests.post(url, json=payload, headers=headers)

print(response.text)

```

**Example response**

```json

{
  "task_id": 17,
  "title": "Update POST /task endpoint request body",
  "description": "Complete the request body section for POST/task endpoint.",
  "status": "OPEN",
  "category": "API_DOCS",
  "last_updated": "2025-04-03",
  "connected_tasks": [
    {
      "task_id": 12,
      "title": "Validate Technical Writer Tasks API OAS",
      "description": "Correct errors in the existing TWT API OAS to validate the specification file.",
      "status": "OPEN"
    }
  ]
}

```

<br />

## Best practices

To increase your rate of success with the TWT API, please adhere to the following best practices.

### Rate limiting

API rate limits regulate the number of calls made with a specific API key. You can manage your rate limits by:

* Distributing the number of requests over time.
* Caching frequent results to reduce the number of API calls.
* Monitoring for `429` responses and retrying requests responsibly.

### Error handling

Table with included errors with example and remediation steps

| Error Code | Name               | Description                                                             | Remediation Steps                                                                                                         |
| :--------- | :----------------- | :---------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| 400        | Bad Request        | The request does not match the expected schema or has incorrect fields. | Ensure the request body matches what is expected in the endpoint specification and that all fields are accurate.          |
| 401        | Unauthorized       | The API key is missing or invalid.                                      | Ensure that the X-API-KEY value is both valid and properly input into the request header.                                 |
| 404        | Resource Not Found | The requested resource cannot be found.                                 | Ensure there are no typos in the request URI.                                                                             |
| 429        | Too Many Requests  | This endpoint has received too many requests from the current API key.  | The request limit has been exceeded for this time period. Send another request when the rate limit window has been reset. |

### API Key security

Fireblocks will issue your initial API key. Afterwards, you can generate a new key in your developer portal.

To keep your key secure:

* Keep your key in a safe place.
* Do NOT hardcode it into any calls or code your create.
* Regenerate your API key if it becomes compromised.
