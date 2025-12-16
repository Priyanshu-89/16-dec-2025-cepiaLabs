# Backend Fundamentals: Zod, Authentication, MongoDB & Mongoose

---

## zod Validation

### 1.  What problem does Zod solve in a backend application? Why not rely only on frontend
validation?
zod provides **runime validation** and **type safety** for incoming backend data like: request body, params Frontend validation alone is not enough because:
- Frontend can be bypassed
- Backend needs **truested, validated data** before database operations

---

### 2.  What is the difference between z.string() and z.string().min(3)?

- `z.string()` validates that value is a string
- `z.string().min(3)` → validates that the value is a string **with at least 3 characters**

---

### 3.  What happens if Zod validation fails? How do you send the error to the client?
When validation fails:

-zod throws an error (with `.parse()`)
-or returns a failure object (with `.safeParse()`)

---

### 4. Explain the difference between .parse() and.safeParse().

| Method | Behavior |
|------|---------|
| `.parse()` | Throws an exception if validation fails |
| `.safeParse()` | Returns `{ success: false, error }` without throwing |

**Conclusion:**  
Use `.safeParse()` for clean, controlled error handling in APIs.

---

### 5. How do you validate email and password using Zod? Explain rules.

```ts
z.object({
email:z.string().email(),
password:z.string().min(8)
});

```
# Rules: 

- Email must be in valid email format
- Password must be at least 8 characters long
- Regex can be added for stronger password rules (uppercase, symbols, special characters, etc).

---

### 6. What is z.object() and why is nested validation important?

 z.object() is used to validate structures JSON-like data.

- Example:
```ts
z.object({
user:z.object({
name:z.string(),
age:z.number()
})
})
```

 Why nested validation matters:
- Ensures deep data integrity
- Prevents invalid nested objects
- Essentail for complex APIs and real-world data structures
  
---

### 7. Why is Zod better than manual if-else validation?

- Cleaner and declarative syntax
- Centralized validation logic
- Automatic TypeScript type inference
- Better and structured error messages
- Less bug-prone and more scalable
  
---

## AUTHENTICATION

### 8. What is authentication? How is it different from authorization?

**Authentication** → Who are you?  
**Authorization** → What are you allowed to do?


Example: 
- Login -> Authentication
- Admin access -> Authorization

  
---

### 9. Why should passwords never be stored in plain text? Name one hashing library.

# Reasons: 
- Plain text passwords can be leaked in database breaches
- One leak can compromise multiple user accounts
  
# Hashing libraries
- bcrypt

---

### 10. What is JWT? Name its three parts and what each part stores.

JWT (JSON Web Token) is a secure token used for authentication.

- Parts of JWT:
  1. **Header** → Algorithm & token type
2. **Payload** → User data (id, role)
3. **Signature** → Verifies integrity


---

### 11. Where should JWT be stored on the client? Which option is dangerous and why?

Best option:

- HTTP-only cookies (protected from JavaScript access)

Dangerous option:

- localStorage (vulnerable to XSS attacks)

---

### 12. Explain the backend login flow step by step.

  1. User sends email and password
  2. Validate input using Zod
  3. Find user in the database
  4. Compare password using bcrypt
  5. Generate JWT
  6. Send token to client (cookie or response)

---

### 13. What is token expiration and why is it important?

- Token expiration:
  Limits how long a token is valid
  Redues damage if a token is stolen
  Forces periodic re-authentication

  ```ts
  expiresIn: "1h"

  ```
  ---

  ### 14. What is authentication middleware? Give one use case.

  Authentication middleware:

  - Verifies JWT
  - Attaches user data to the request object

  Use case:

  - Protecting private routes like /dashboard
    
---

## MONGODB & MONGOOSE

### 15. What is MongoDB? How is it different from SQL databases?

MongoDB:

- NoSQL database

- Document-based (JSON-like)

- Flexible schema

SQL Databases:

- Table-based

- Fixed schema

- Relational structure

---

### 16. What is a Mongoose schema and why is it required?

A Mongoose schema defines:

- Data Structure
- Field types
- Validation rules
- Constraints

# It ensures data consistency and validation in MongoDB collections.

---

### 17. What does unique: true do? Does it guarantee uniqueness?

- Creates a unique index in MongoDB
- Prevents duplicate values

# It does not fully guarantee uniqueness due to possible race conditions.

---

### 18. Difference between findOne() and findByld()?

| Method      | Usage                                      |
|------------|---------------------------------------------|
| `findOne()` | Finds a document using any condition        |
| `findById()`| Finds a document using `_id` only            |


---

### 19. Why are indexes important in MongoDB?

Indexes:

- Improve query performance
- Reduce full collection scans
- Speed up search, filtering, and sorting

---

### 20. Why do we use .env files for MongoDB connection? What happens if pushed to GitHub?

.env files stores:

- Database connection URLs
- Secrets and credentails

If pushed to GitHub:

- Credentials can be leaked
- Database may be hacked
- Serious security risk
  
---











