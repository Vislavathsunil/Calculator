# User Registration Endpoint Documentation

## Endpoint: `/users/register`

### Method: POST

### Description:

This endpoint is used to register a new user. It validates the input data, hashes the password, creates a new user in the database, and returns a JSON Web Token (JWT) along with the user details.

### Request Body:

The request body should be a JSON object containing the following fields:

- `fullname`: An object containing:
  - `firstname`: A string with a minimum length of 3 characters.
  - `lastname`: A string with a minimum length of 3 characters.
- `email`: A string representing a valid email address.
- `password`: A string with a minimum length of 6 characters.

### Example Request:
```json
{
"fullname": {
"firstname": "John",
"lastname": "Doe"
},
"email": "john.doe@example.com",
"password": "password123"
}
```

### Example Response
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2N2E5YWQ0Y2QwY2ZmNTcyYjliZTYxMzkiLCJpYXQiOjE3MzkxNzMxOTYsImV4cCI6MTczOTI1OTU5Nn0.FA90P6FELOKi4exzQaKGDc9yfRDaJCOyvhNpiF8aWJc",
    "user": {
        "fullName": {
            "firstname": "Hero",
            "lastname": "Nayak"
        },
        "email": "hero@gmail.com",
        "password": "$2b$10$e94qpZj2YPxOd5gC3js4DuXvp3QhB8oyAx8/2byqQkCRQPj3sJVIW",
        "_id": "67a9ad4cd0cff572b9be6139",
        "__v": 0
    }
}
```

## `/user/login` Endpoint

This endpoint authenticates an existing user by validating the provided credentials, compares the given password with the stored hash, and returns a JWT upon successful authentication.

### Functionality

- **Input Validation:**  
  Uses [express-validator](https://express-validator.github.io/) to ensure:

  - `email` is a valid email address.
  - `password` has a minimum length of 6 characters.

- **Password Verification:**  
  Searches for the user by email and uses the user model's `comparePassword` function to verify the provided password against the stored hashed password.

- **Token Generation:**  
  Upon successful verification, generates a JWT for the authenticated user using the `generateAuthToken` method.

- **Response Handling:**
  - On success, responds with status 200 and returns the JWT token along with user details.
  - If credentials are invalid, returns a 401 status with an error message.
  - If input validation fails, returns a 400 status with details of the validation errors.

### Request

- **Method:** POST
- **URL:** `/user/login`
- **Headers:**  
  `Content-Type: application/json`

- **Request Body:**  
```json
  {
  "email": "john.doe@example.com",
  "password": "password123"
  }
```

### Example Response

- `users` (object):
- `fullname` (object).
- `firstnames` (string): User's first name (minimum 3 characters).
- `Slastname` (string): User's last name (minimum 3 characters).
- `email` (string): User's email address (must be a valid email).
- `password` (string): User's password (minimum 6 characters).
- `token` (String): JWT Token

```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2N2E2MTI2ZDA4NGI5NjNjYThjMzJlNWMiLCJpYXQiOjE3MzkxNzMyNzAsImV4cCI6MTczOTI1OTY3MH0.HthxwPD73uXKLXsjnH5DNQAo8XVqmuoqnm3ET9G4LEU",
    "user": {
        "fullName": {
            "firstname": "Power",
            "lastname": "Nayak"
        },
        "_id": "67a6126d084b963ca8c32e5c",
        "email": "Nayak@gmail.com",
        "password": "$2b$10$Yg4KZD1c8FMJrs9mtHLEeO1hLkuRs4vdIfq1iRc1nAHG0xacvoIgm",
        "__v": 0
    }
}
```


## `/users/profile` Endpoint

This endpoint returns the authenticated user's profile details. It requires the user to be authenticated via a valid JWT token, which should be sent either as a cookie or in the Authorization header.

### Functionality

- **Authentication:**  
  Uses the `authMiddleware.authUser` to verify the JWT token provided in the request.
- **Profile Retrieval:**  
  Once authenticated, the endpoint returns the user data stored in `req.user`.

### Request

- **Method:** GET
- **URL:** `/users/profile`
- **Headers:**  
  - `Content-Type: application/json`  
  - `Authorization: Bearer <jwt_token>` *(if not using cookies)*

### Example Response
 
- `users` (object):
  - `fullname` (object).
     - `firstnames` (string): User's first name (minimum 3 characters).
      - `lastname` (string): User's last name (minimum 3 characters).
  - `email` (string): User's email address (must be a valid email).

- **Success (200 OK):**


## `/users/logout` Endpoint

This endpoint logs out the authenticated user by clearing the authentication token stored in cookies and adding that token to a blocklist to prevent further use.

### Functionality

- **Authentication:**  
  Uses the `authMiddleware.authUser` to verify the JWT token provided in the request.
- **Token Clearing:**  
  Clears the token from cookies.
- **Token Invalidation:**  
  Stores the token in a blocklist (via `blocklistTokenModel`) so that it cannot be reused.

### Request

- **Method:** GET
- **URL:** `/users/logout`
- **Headers:**  
  - `Content-Type: application/json`  
  - `Authorization: Bearer <jwt_token>` *(if the token is not provided via cookies)*

### Example Response
  - {
  -   "message": "Logged Out"
  - }

- **Success (200 OK):**


// ...existing content...

## `/captain/register` Endpoint

### Method: POST

### Description
This endpoint is used to register a new captain. It validates the input data, hashes the captain's password, creates a captain in the database, and returns a token with the captain details.

### Request Body
- **fullname:**  
  An object containing:  
  - **firstname** (string, minimum length 3)  
  - **lastname** (string, minimum length 3)  

- **email** (string, valid email)  
- **password** (string, minimum length 6)  
- **vehicle** (object):
  - **color** (string, minimum length 3)  
  - **plate** (string, minimum length 3)  
  - **capacity** (integer, at least 1)  
  - **vehicleType** (enum: ["motorcycle", "auto", "car"])

### Example Request
```json
{
  "fullname": {
    "firstname": "Jane",
    "lastname": "Doe"
  },
  "email": "jane.doe@example.com",
  "password": "mypassword123",
  "vehicle": {
    "color": "Red",
    "plate": "XYZ123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

### Example Response
```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2N2E5YWU2MWQwY2ZmNTcyYjliZTYxM2QiLCJpYXQiOjE3MzkxNzM0NzMsImV4cCI6MTczOTI1OTg3M30.QwmaM6ZyPrUgzPrsPrv7IRb5qGTvRXKxxs6g2n6ipH8",
    "captain": {
        "fullname": {
            "firstname": "Ravi",
            "lastname": "Charan"
        },
        "email": "ravicharan@gmail.com",
        "password": "$2b$10$D6kUD4UXCnPdaA26HHZtRuNcq/5j7TIg054jk1mOpEoa2mNRCWMTS",
        "status": "inactive",
        "vehicle": {
            "color": "white",
            "plate": "MH 12 KB 1232",
            "capacity": 6,
            "vehicleType": "car"
        },
        "_id": "67a9ae61d0cff572b9be613d",
        "__v": 0
    }
}
```


   
