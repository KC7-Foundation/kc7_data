
# KC7 API Documentation

## Overview
This document provides detailed information about the endpoints available in the KC7 API. The API includes routes for user authentication, retrieving user information, managing user progress in modules, and more.

## Endpoints

### General Endpoints

#### `GET /api`
- **Description**: API endpoint for testing purposes.
- **Returns**: A JSON response with a greeting message.
- **Response Example**:
  
  ```json
  {
    "message": "Hello, World!"
  }
  ```

#### `GET /api/docs`
- **Description**: API documentation page.
- **Returns**: Renders the API documentation page.

## Authentication Endpoints

#### `POST /api/auth`
- **Description**: Authenticate a user and return a JWT token.
- **Request Body**:
  
  ```json
  {
    "username": "string",
    "password": "string"
  }
  ```
- **Returns**: A JSON dictionary containing the JWT token if authentication is successful, or an error message if not.
- **Response Example**:
  
  ```json
  {
    "access_token": "your_jwt_token_here"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "Invalid credentials"
  }
  ```

## User Endpoints

#### `GET /api/users/me`
- **Description**: Retrieve the information of the current user.
- **Authorization**: Requires a valid JWT token.
- **Returns**: A JSON dictionary containing the user's ID and username.
- **Response Example**:
  
  ```json
  {
    "user_id": 1,
    "username": "example_user"
  }
  ```

#### `GET /api/users/<string:username>`
- **Description**: Retrieve a user's information by their username.
- **Authorization**: Requires a valid JWT token.
- **Path Parameters**:
  - `username`: The username of the user to retrieve.
- **Returns**: A JSON dictionary containing the user's ID and username, or an error message if the user is not found.
- **Response Example**:
  
  ```json
  {
    "user_id": 1,
    "username": "example_user"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "User not found"
  }
  ```

#### `GET /api/users/<int:user_id>`
- **Description**: Retrieve a user's information by their ID.
- **Authorization**: Requires a valid JWT token.
- **Path Parameters**:
  - `user_id`: The ID of the user to retrieve.
- **Returns**: A JSON dictionary containing the user's ID and username, or an error message if the user is not found.
- **Response Example**:
  
  ```json
  {
    "user_id": 1,
    "username": "example_user"
  }
  ```
- **Error Response Example**:
  ```json
  {
    "message": "User not found"
  }
  ```

#### `GET /api/users/email/<string:email>`
- **Description**: Retrieve a user's information by their email address.
- **Authorization**: Requires a valid JWT token. Only users with 'Admin', 'Manager', or 'API' roles can access this endpoint.
- **Path Parameters**:
  - `email`: The email address of the user to retrieve.
- **Returns**: A JSON dictionary containing the user's ID, or an error message if the user is not found or unauthorized.
- **Response Example**:

  ```json
  {
    "user_id": 1
  }
  ```
- **Error Response Example**:

  ```json
  {
    "message": "User not found"
  }
  ```

  ```json
  {
    "message": "Unauthorized"
  }
  ```

#### `POST /api/users`
- **Description**: Create a new user via the API.
- **Authorization**: Requires a valid JWT token and is rate-limited to 5 requests per minute.
- **Request Body**:
  
  ```json
  {
    "username": "string",
    "email": "string"
  }
  ```
- **Returns**: A JSON dictionary containing the details of the created user, or an error message if the request fails.
- **Response Example**:
  
  ```json
  {
    "user_id": 123,
    "username": "example_user",
    "email": "user@example.com",
    "created_at": "2024-07-30T12:34:56.789Z"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "Email is required"
  }
  ```

## Module Endpoints

#### `GET /api/modules`
- **Description**: Retrieve a catalog of core modules.
- **Returns**: A JSON list of core modules.
- **Response Example**:
  
  ```json
  [
    {
      "id": 1,
      "title": "Module Title",
      "clean_title": "Module Title",
      "description": "Module Description",
      "est_time": "na",
      "difficulty": "Easy"
    }
  ]
  ```

#### `GET /api/users/<int:user_id>/modules`
- **Description**: Retrieve a user's progress in all core modules.
- **Authorization**: Requires a valid JWT token.
- **Path Parameters**:
  - `user_id`: The unique identifier of the user.
- **Returns**: A JSON list of dictionaries containing the module ID, module name, user's score, max score, and progress percentage for each module, or an error message if the user is not found.
- **Response Example**:
  
  ```json
  [
    {
      "module_id": 1,
      "module_name": "Module Title",
      "score": 80,
      "max_score": 100,
      "progress": 0.8
    }
  ]
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "User not found"
  }
  ```

#### `GET /api/users/<int:user_id>/modules/<int:module_id>`
- **Description**: Retrieve a user's progress in a specific module.
- **Authorization**: Requires a valid JWT token.
- **Path Parameters**:
  - `user_id`: The unique identifier of the user.
  - `module_id`: The unique identifier of the module.
- **Returns**: A JSON dictionary containing the user ID, module ID, user's score, max score, and progress percentage, or an error message if the user or module is not found or the user is not enrolled in the module.
- **Response Example**:
  
  ```json
  {
    "user_id": 1,
    "module_id": 1,
    "score": 80,
    "max_score": 100,
    "progress": 0.8
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "User not found"
  }
  ```

## Authentication with Tokens

#### `GET /auth/token-login`
- **Description**: Authenticate a user using a temporary token.
- **Query Parameters**:
  - `token`: The temporary token for authentication.
- **Returns**: A JSON dictionary containing the access token, or an error message if the token is invalid or expired.
- **Response Example**:
  
  ```json
  {
    "access_token": "your_access_token_here"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "Invalid token"
  }
  ```

#### `POST /auth/refresh-token`
- **Description**: Refresh the access token using the refresh token.
- **Request Cookies**:
  - `refresh_token`: The refresh token stored in the HttpOnly cookie.
- **Returns**: A JSON dictionary containing the new access token, or an error message if the refresh token is invalid or expired.
- **Response Example**:
  
  ```json
  {
    "access_token": "your_new_access_token_here"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "Invalid refresh token"
  }
  ```

#### `GET /auth/magic-link`
- **Description**: Authenticate a user using a temporary token and log them in.
- **Query Parameters**:
  - `token`: The temporary token for authentication.
- **Returns**: Redirects to the dashboard or the next URL if provided, or an error message if the token is invalid or expired.

#### `POST /api/users/<int:user_id>/magic-link`
- **Description**: Generate a one-time login link for a user.
- **Authorization**: Requires a valid JWT token and the user must have 'Admin', 'Manager', or 'API' role.
- **Path Parameters**:
  - `user_id`: The unique identifier of the user.
- **Request Headers**:
  - `X-Integration-Key`: The integration key for authentication.
- **Returns**: A JSON dictionary containing the one-time login link, or an error message if the user is not found or unauthorized.
- **Response Example**:
  
  ```json
  {
    "login_link": "http://apidomain/auth/magic-link?token=your_temp_token_here"
  }
  ```
- **Error Response Example**:
  
  ```json
  {
    "message": "User not found"
  }
  ```
