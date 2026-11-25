# RESTful API Testing Documentation – Booking & Payment Service

**API Base URL:** `https://herokuapp.com`  
**Tester:** Alondra Valdez  
**Tools:** Postman, JSON Schema Validation, Chrome DevTools  

---

## 1. API Test Cases Summary

| Test ID | Endpoint | Method | Test Scenario | Expected Status | Validation / Assertions | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **API-01** | `/auth` | `POST` | Generate authentication token with valid admin credentials | `200 OK` | Response JSON contains a non-empty `token` string. | **Pass** |
| **API-02** | `/auth` | `POST` | Attempt authentication with invalid password | `200 OK` | Response returns `{"reason": "Bad credentials"}` without token. | **Pass** |
| **API-03** | `/booking` | `POST` | Create a new booking with valid customer and payment data (`totalprice`, `depositpaid`) | `200 OK` | Returns unique `bookingid` and echoes exact `totalprice` and `depositpaid` boolean. | **Pass** |
| **API-04** | `/booking/:id` | `GET` | Retrieve created booking details by ID | `200 OK` | Response time `< 800ms`; fields match created record (`firstname`, `totalprice`). | **Pass** |
| **API-05** | `/booking/:id` | `PUT` | Update booking price without authentication token | `403 Forbidden` | Request is rejected; unauthorized user cannot alter payment amounts. | **Pass** |
| **API-06** | `/booking/:id` | `POST` | Create booking with negative `totalprice` (`-150`) | `400 Bad Request` | API should reject negative financial amounts. | **Fail** *(See BUG-API-01)* |
| **API-07** | `/booking/999999` | `GET` | Query non-existent booking ID | `404 Not Found` | Returns `Not Found` message cleanly without server crash (`500`). | **Pass** |

---

## 2. API Defect Report

### BUG-API-01: `POST /booking` accepts negative values in `totalprice` field
- **Severity:** High | **Priority:** High | **Status:** Open
- **Endpoint:** `POST https://herokuapp.com/booking`
- **Payload Sent:**
  ```json
  {
    "firstname": "Alondra",
    "lastname": "Valdez",
    "totalprice": -150,
    "depositpaid": true,
    "bookingdates": { "checkin": "2026-12-01", "checkout": "2026-12-05" }
  }
  ```
- **Expected Result:** API returns `400 Bad Request` with validation error indicating `totalprice` must be greater than `0`.
- **Actual Result:** API returns `200 OK` and persists `-150` as a valid transaction amount, creating financial reconciliation risks.
