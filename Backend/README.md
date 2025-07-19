# RideX Backend API Documentation

## `/users/register` Endpoint

### **Description**
Registers a new user in the RideX system.  
This endpoint validates the input, checks for existing users, hashes the password, and creates a new user record.

---

### **Method**
`POST`

---

### **Request Body Format**

Send a JSON object with the following structure:

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "yourpassword"
}
```

#### **Field Requirements**
- `fullname.firstname` (string, required, min 3 characters)
- `fullname.lastname` (string, optional, min 3 characters if provided)
- `email` (string, required, must be a valid email)
- `password` (string, required, min 6 characters)

---

### **Responses**

#### **201 Created**
```json
{
  "token": "<JWT Token>",
  "user": {
    "_id": "...",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
}
```

#### **400 Bad Request**
- Validation errors or user already exists.
```json
{
  "errors": [
    {
      "msg": "First name must be at least 3 characters long",
      "param": "fullname.firstname",
      "location": "body"
    }
  ]
}
```
or
```json
{
  "message": "User already exist"
}
```

---

### **Example Request (curl)**
```bash
curl -X POST http://localhost:4000/users/register \
  -H "Content-Type: application/json" \
  -d '{
    "fullname": { "firstname": "John", "lastname": "Doe" },
    "email": "john.doe@example.com",
    "password": "yourpassword"
  }'
```

---

### **Notes**
- On success, a JWT token is returned for authentication.
- All required fields must be present and valid.

---

## `/users/login` Endpoint

### **Description**
Authenticates a user and returns a JWT token if credentials are valid.

---

### **Method**
`POST`

---

### **Request Body Format**

Send a JSON object with the following structure:

```json
{
  "email": "john.doe@example.com",
  "password": "yourpassword"
}
```

#### **Field Requirements**
- `email` (string, required, must be a valid email)
- `password` (string, required, min 6 characters)

---

### **Responses**

#### **200 OK**
```json
{
  "token": "<JWT Token>",
  "user": {
    "_id": "...",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
}
```

#### **400 Bad Request**
- Validation errors.
```json
{
  "errors": [
    {
      "msg": "Invalid Email",
      "param": "email",
      "location": "body"
    }
  ]
}
```

#### **401 Unauthorized**
- Invalid email or password.
```json
{
  "message": "Invalid email or password"
}
```

---

### **Example Request (curl)**
```bash
curl -X POST http://localhost:4000/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john.doe@example.com",
    "password": "yourpassword"
  }'
```

---

### **Notes**
- On success, a JWT token is returned for authentication.
- The token is also set as a cookie named `token`.
- All required fields must be present


---


## `/users/profile` Endpoint

### **Description**
Returns the authenticated user's profile information.  
Requires a valid JWT token (sent as a cookie or in the `Authorization` header).

---

### **Method**
`GET`

---

### **Headers**
- `Authorization: Bearer <JWT Token>` (if not using cookies)

---

### **Responses**

#### **200 OK**
```json
{
  "_id": "...",
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "socketId": "..."
}
```

#### **401 Unauthorized**
```json
{
  "message": "Unauthorized"
}
```

---

### **Example Request (curl)**
```bash
curl -X GET http://localhost:4000/users/profile \
  -H "Authorization: Bearer <JWT Token>"
```

---

## `/users/logout` Endpoint

### **Description**
Logs out the authenticated user by blacklisting their JWT token and clearing the cookie.

---

### **Method**
`GET`

---

### **Headers**
- `Authorization: Bearer <JWT Token>` (if not using cookies)

---

### **Responses**

#### **200 OK**
```json
{
  "message": "Logged out"
}
```

#### **401 Unauthorized**
```json
{
  "message": "Unauthorized"
}
```

---

### **Example Request (curl)**
```bash
curl -X GET http://localhost:4000/users/logout \
  -H "Authorization: Bearer <JWT Token>"
```

---

### **Notes**
- The token is blacklisted and cannot be used again.
- The cookie named `token` is cleared on

---


## Captain Endpoints

### `/captains/register`

**Description:**  
Registers a new captain (driver) with vehicle details.

**Method:**  
`POST`

**Request Body Format:**
```json
{
  "fullname": {
    "firstname": "Jane",
    "lastname": "Smith"
  },
  "email": "jane.smith@example.com",
  "password": "yourpassword",
  "vehicle": {
    "color": "Red",
    "plate": "ABC123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

**Field Requirements:**
- `fullname.firstname` (string, required, min 3 characters)
- `fullname.lastname` (string, optional, min 3 characters if provided)
- `email` (string, required, must be a valid email)
- `password` (string, required, min 6 characters)
- `vehicle.color` (string, required, min 3 characters)
- `vehicle.plate` (string, required, min 3 characters)
- `vehicle.capacity` (integer, required, min 1)
- `vehicle.vehicleType` (string, required, one of: `car`, `motorcycle`, `auto`)

**Responses:**

- **201 Created**
  ```json
  {
    "token": "<JWT Token>",
    "captain": {
      "_id": "...",
      "fullname": { "firstname": "Jane", "lastname": "Smith" },
      "email": "jane.smith@example.com",
      "vehicle": {
        "color": "Red",
        "plate": "ABC123",
        "capacity": 4,
        "vehicleType": "car"
      }
    }
  }
  ```

- **400 Bad Request**
  - Validation errors or captain already exists.
  ```json
  {
    "errors": [
      {
        "msg": "First name must be at least 3 characters long",
        "param": "fullname.firstname",
        "location": "body"
      }
    ]
  }
  ```
  or
  ```json
  {
    "message": "Captain already exist"
  }
  ```

**Example Request (curl):**
```bash
curl -X POST http://localhost:4000/captains/register \
  -H "Content-Type: application/json" \
  -d '{
    "fullname": { "firstname": "Jane", "lastname": "Smith" },
    "email": "jane.smith@example.com",
    "password": "yourpassword",
    "vehicle": {
      "color": "Red",
      "plate": "ABC123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }'
```

---

### `/captains/login`

**Description:**  
Authenticates a captain and returns a JWT token if credentials are valid.

**Method:**  
`POST`

**Request Body Format:**
```json
{
  "email": "jane.smith@example.com",
  "password": "yourpassword"
}
```

**Field Requirements:**
- `email` (string, required, must be a valid email)
- `password` (string, required, min 6 characters)

**Responses:**

- **200 OK**
  ```json
  {
    "token": "<JWT Token>",
    "captain": {
      "_id": "...",
      "fullname": { "firstname": "Jane", "lastname": "Smith" },
      "email": "jane.smith@example.com",
      "vehicle": {
        "color": "Red",
        "plate": "ABC123",
        "capacity": 4,
        "vehicleType": "car"
      }
    }
  }
  ```

- **400 Bad Request**
  - Validation errors.
  ```json
  {
    "errors": [
      {
        "msg": "Invalid Email",
        "param": "email",
        "location": "body"
      }
    ]
  }
  ```

- **401 Unauthorized**
  - Invalid email or password.
  ```json
  {
    "message": "Invalid email or password"
  }
  ```

**Example Request (curl):**
```bash
curl -X POST http://localhost:4000/captains/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane.smith@example.com",
    "password": "yourpassword"
  }'
```

---

### `/captains/profile`

**Description:**  
Returns the authenticated captain's profile information.  
Requires a valid JWT token (sent as a cookie or in the `Authorization` header).

**Method:**  
`GET`

**Headers:**
- `Authorization: Bearer <JWT Token>` (if not using cookies)

**Responses:**

- **200 OK**
  ```json
  {
    "captain": {
      "_id": "...",
      "fullname": { "firstname": "Jane", "lastname": "Smith" },
      "email": "jane.smith@example.com",
      "vehicle": {
        "color": "Red",
        "plate": "ABC123",
        "capacity": 4,
        "vehicleType": "car"
      },
      "status": "active",
      "socketId": "...",
      "location": {
        "ltd": 12.34,
        "lng": 56.78
      }
    }
  }
  ```

- **401 Unauthorized**
  ```json
  {
    "message": "Unauthorized"
  }
  ```

**Example Request (curl):**
```bash
curl -X GET http://localhost:4000/captains/profile \
  -H "Authorization: Bearer <JWT Token>"
```

---

### `/captains/logout`

**Description:**  
Logs out the authenticated captain by blacklisting their JWT token and clearing the cookie.

**Method:**  
`GET`

**Headers:**
- `Authorization: Bearer <JWT Token>` (if not using cookies)

**Responses:**

- **200 OK**
  ```json
  {
    "message": "Logout successfully"
  }
  ```

- **401 Unauthorized**
  ```json
  {
    "message": "Unauthorized"
  }
  ```

**Example Request (curl):**
```bash
curl -X GET http://localhost:4000/captains/logout \
  -H "Authorization: Bearer <JWT Token>"
```

---

**Notes:**
- On logout, the token is blacklisted and cannot be used again.
- The cookie named `token` is cleared