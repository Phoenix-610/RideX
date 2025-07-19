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
    "email": "john.doe@example.com",
    // other user fields
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
    // ...other errors
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
- All required fields must be present and