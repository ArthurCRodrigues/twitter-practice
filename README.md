# Twitter Backend API Simulation Challenge
This challenge focuses on simulating a simplified Twitter API backend. Your goal is to implement a REST API that handles tweets and provides basic tweet management.

## 1. Introduction
Your mission is to create a REST API that manages tweets. For this challenge, you can use any backend language or framework you prefer, but the core principles of a RESTful API should be followed.

## 2. Challenge Definition
In this challenge, you MUST create a REST API to simulate a Twitter-like service. Pay close attention to all the following instructions!

### 2.1. Technical Restrictions
Your project:

- MUST store all data in memory. No databases (SQL, NoSQL, etc.) or caching systems (Redis, Memcached, etc.) are allowed.

- MUST accept and respond only with JSON.

- SHOULD have a clear and organized project structure.

- SHOULD include unit tests for your API logic, even if simplified.

- MUST NOT explore file storage, user authentication, or authorization. All operations are assumed to be public or handled by a generic "system" user for the purpose of this challenge.

## 2.2. API Endpoints
The following endpoints must be present in your API, along with their expected functionality.

### 2.2.1. Create a Tweet: POST /tweets
This endpoint will receive new tweets. Each tweet consists of a content and an authorHandle (representing the Twitter handle of the "user" who posted it).

**JSON**
```
{
    "content": "This is my first tweet!",
    "authorHandle": "@my_awesome_user"
}
```
The fields in the JSON above mean the following:

- ```content```: Text content of the tweet. **REQUIRED**

- ```authorHandle```: Twitter handle of the author (e.g., @user). **REQUIRED**


**The API will only accept tweets that:**

- Have both content and authorHandle filled.

- content MUST NOT be empty or just whitespace.

- content MUST NOT exceed 280 characters.

- ```authorHandle``` MUST start with @ and contain only alphanumeric characters and underscores (_) after the @. It also MUST NOT be empty after the @.

### As a response, this endpoint is expected to respond with:

```201 Created``` with the created tweet object, including a unique id and a timestamp.
Example Response:

**JSON**
```
{
    "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
    "content": "This is my first tweet!",
    "authorHandle": "@my_awesome_user",
    "timestamp": "2025-06-08T18:37:30.000-03:00"
}
```
(Note: The timestamp should be the time the tweet was received by the API, in ISO 8601 format.)

- ```422 Unprocessable Entity``` without any body. The tweet was not accepted for any reason (one or more of the acceptance criteria were not met - e.g., content too long, invalid handle).

- ```400 Bad Request``` without any body. The API did not understand the client's request (e.g., an invalid JSON structure, missing required fields entirely).

### 2.2.2. Get Tweets from an Author: GET /tweets/{authorHandle}
This endpoint should return a list of tweets by a specific author.

**Example Request:** ```GET /tweets/@my_awesome_user```

As a response, this endpoint is expected to respond with:

```200 OK``` with a JSON array of tweet objects belonging to the specified ```authorHandle```. The tweets should be ordered from **most recent to oldest**.
Example Response (if @my_awesome_user has posted two tweets):

JSON
```
[
    {
        "id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "content": "Another great day!",
        "authorHandle": "@my_awesome_user",
        "timestamp": "2025-06-08T18:40:00.000-03:00"
    },
    {
        "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
        "content": "This is my first tweet!",
        "authorHandle": "@my_awesome_user",
        "timestamp": "2025-06-08T18:37:30.000-03:00"
    }
]
```

```404 Not Found``` without any body. If no tweets are found for the given authorHandle.

```400 Bad Request``` without any body. If the authorHandle in the path is not a valid format (e.g., doesn't start with @).

### 2.2.3. Get All Tweets: GET /tweets
This endpoint should return a list of all tweets in the system, ordered from most recent to oldest.

As a response, this endpoint is expected to respond with:

```200 OK``` with a JSON array of all tweet objects.
Example Response (if there are two tweets in the system):

JSON
```
[
    {
        "id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "content": "Another great day!",
        "authorHandle": "@my_awesome_user",
        "timestamp": "2025-06-08T18:40:00.000-03:00"
    },
    {
        "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
        "content": "This is my first tweet!",
        "authorHandle": "@my_awesome_user",
        "timestamp": "2025-06-08T18:37:30.000-03:00"
    }
]
```
An empty array [] should be returned if no tweets exist.

### 2.2.4. Delete a Tweet: DELETE /tweets/{id}
This endpoint deletes a tweet by its unique id.

Example Request: ```DELETE /tweets/a1b2c3d4-e5f6-7890-1234-567890abcdef```

As a response, this endpoint is expected to respond with:

```204 No Content``` without any body. The tweet was successfully deleted.

```404 Not Found``` without any body. If the tweet with the given id does not exist.
