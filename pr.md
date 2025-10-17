# feat: Implement user registration form validation


### 1. CONTEXT AND MOTIVATION

This change is necessary because the user registration form currently allows account creation with incomplete or incorrectly formatted data. This leads to database inconsistencies and a poor user experience.

This PR **addresses** `issue #123: Validate input data in registration form`, tackling this critical vulnerability and **resolving** the need to ensure data integrity at the entry point.

---

### 2. DESCRIPTION OF THE SOLUTION

This PR **implements** a robust validation layer for the user registration form. It **introduces** the following changes:

* **Adds** validation for the `email` field to ensure a valid email format.
* **Adds** validation for the `password` field, requiring a minimum of 8 characters, at least one uppercase letter, one number, and one symbol.
* **Modifies** the `/api/register` endpoint to apply these validation rules before attempting to store the user.
* **Updates** the error messages returned to the client to be more specific in case of validation failures.

---

### 3. JUSTIFICATION AND BENEFITS

This solution significantly **improves** the quality of user data stored in the system. By validating data on the server, it **enhances** the application's overall security and **ensures** that only users with valid information are created.

* It **optimizes** the registration process by reducing manual errors and the need for subsequent corrections.
* It **enables** a better end-user experience by providing clear and immediate feedback on input errors.

---

### 4. EVIDENCE AND TESTING

The changes have been  **tested** with the following tests:

* **Unit Tests:** New unit tests have been **added** for the `email` and `password` validation functions in `userValidation.test.js`, **validating** that the rules are applied correctly.
* **Integration Tests:** Integration tests were run simulating registration attempts with both valid and invalid data.
    * `POST /api/register` with an invalid email: **passes** with a 400 error and an `Invalid email` message.
    * `POST /api/register` with a weak password: **passes** with a 400 error and a `Password must contain...` message.
    * `POST /api/register` with valid data: **passes** successfully, creating the user.

Below is the output from the unit tests:

```bash
$ npm test userValidation.test.js

> user-registration-api@1.0.0 test
> jest userValidation.test.js

 PASS  tests/userValidation.test.js
  User Validation Tests
    Email Validation
      ✓ should accept valid email format (12 ms)
      ✓ should reject invalid email format (8 ms)
    Password Validation
      ✓ should accept password with minimum 8 characters, uppercase, number and symbol (15 ms)
      ✓ should reject password shorter than 8 characters (7 ms)
      ✓ should reject password without required complexity (10 ms)

Test Suites: 1 passed, 1 total
Tests:       5 passed, 5 total
Snapshots:   0 total
Time:        1.842 s, estimated 2 s
Ran all test suites matching /userValidation.test.js.
```

---

### 5. CALL TO ACTION

**Please review** this PR, paying special attention to the validation logic in `src/middlewares/userValidation.js` and how it is **introduced** into the authentication router.

**Feedback is welcome** regarding the clarity of the error messages.

If everything **looks good (LGTM)**, this PR is **ready to merge**.
