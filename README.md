# Module Lab: Building Full CRUD RESTful APIs with Flask

## Learning Goals

- Implement RESTful API endpoints using Flask.
- Handle HTTP POST, PATCH, and DELETE methods to manage resource data.
- Accept and process JSON input using `request.get_json()`.
- Simulate persistent data using in-memory Python objects.
- Follow RESTful route conventions and return structured JSON responses.

## Introduction

In this lab, you will build a **Full CRUD API** to manage a list of events. The API will allow users to:

- Create new events using `POST`
- Update existing events using `PATCH`
- Delete events using `DELETE`

You’ll simulate database-like behavior with in-memory Python class objects and respond to all client requests with properly formatted JSON and appropriate status codes.

This lab reinforces essential backend development skills including route design, data mutation, error handling, and RESTful conventions.

## Setup Instructions

### Fork and Clone the Repository

1. Go to the provided GitHub repository link.
2. Fork the repository to your GitHub account.
3. Clone the forked repository to your local machine:

```bash
git clone <repo-url>
cd course-8-module-5-flask-full-crud-api-lab
```

### Install Dependencies

Ensure Python is installed:

```bash
python --version
```

Install Flask and dependencies using pipenv:

```bash
pipenv install
pipenv shell
```

Or with pip:

```bash
pip install flask
```

## Tasks

### Task 1: Define the Problem

You’re building a basic event management API. It should:

- Accept event creation via `POST /events`
- Allow updating event titles via `PATCH /events/<id>`
- Delete events using `DELETE /events/<id>`
- Respond with structured JSON and appropriate HTTP status codes

---

### Task 2: Determine the Design

The Flask API should be structured as follows:

- Use `@app.route()` with correct HTTP method decorators
- Accept input using `request.get_json()`
- Represent data using a custom `Event` class
- Store events in an in-memory list
- Use `jsonify()` for consistent JSON responses

---

### Task 3: Develop the Code

Create `app.py` and start with the following structure:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# Event class
class Event:
    def __init__(self, id, title):
        self.id = id
        self.title = title

    def to_dict(self):
        return {"id": self.id, "title": self.title}

# In-memory data store
events = [
    Event(1, "Tech Meetup"),
    Event(2, "Python Workshop")
]

# TODO: POST /events - Create a new event from JSON input
# TODO: PATCH /events/<id> - Update the title of an event
# TODO: DELETE /events/<id> - Remove an event from the list

if __name__ == "__main__":
    app.run(debug=True)
```

---

### Task 4: Test the API

Start the Flask development server:

```bash
# Flask Events API

This project is a small Flask API for managing event titles. It demonstrates
REST-style create, update, and delete operations with JSON request and response
bodies.

The API uses an in-memory list instead of a database. It starts with two sample
events, so changes are lost whenever the Flask process restarts.

## Getting Started

Install the dependencies with Pipenv:

```bash
pipenv install
pipenv shell
```

Start the development server:

```bash
python app.py
```

The API is available at `http://localhost:5000`.

## Routes

| Method | Route | Description | Success status |
| --- | --- | --- | --- |
| `POST` | `/events` | Create an event | `201 Created` |
| `PATCH` | `/events/<id>` | Update an event title | `200 OK` |
| `DELETE` | `/events/<id>` | Delete an event | `200 OK` |

There is no `GET` route in the current implementation. The initial events are:

```json
[
  {"id": 1, "title": "Tech Meetup"},
  {"id": 2, "title": "Python Workshop"}
]
```

## Example Requests and Responses

### Create an event

```bash
curl -X POST http://localhost:5000/events \
  -H "Content-Type: application/json" \
  -d '{"title":"Hackathon"}'
```

Response: `201 Created`

```json
{"id": 3, "title": "Hackathon"}
```

### Update an event

```bash
curl -X PATCH http://localhost:5000/events/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Advanced Python Workshop"}'
```

Response: `200 OK`

```json
{"id": 1, "title": "Advanced Python Workshop"}
```

### Delete an event

```bash
curl -X DELETE http://localhost:5000/events/2
```

Response: `200 OK`

```json
{"message":"Event 2 deleted"}
```

## Error Responses

Create and update requests require a non-empty `title` value. Missing or invalid
JSON returns `400 Bad Request`:

```json
{"error":"Title is required"}
```

Updating or deleting an ID that does not exist returns `404 Not Found`:

```json
{"error":"Event not found"}
```

## Tests

Run the automated tests with:

```bash
pytest
```
