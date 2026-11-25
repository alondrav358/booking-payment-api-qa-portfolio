# Booking & Payment REST API QA Portfolio (Postman)

RESTful API testing portfolio created by **Alondra Valdez** demonstrating functional API validation, token-based authentication testing, negative boundary checks, and financial payload verification (`totalprice`, `depositpaid`) using **Postman**.

---

## Repository Contents

| File / Folder | Description |
| :--- | :--- |
| **[`/api-testing/API_Test_Cases.md`](./api-testing/API_Test_Cases.md)** | API test scenarios (`POST`, `GET`, `PUT`), HTTP status validations, and financial boundary defect report. |
| **[`/postman-collection/Booking_Payment_API.postman_collection.json`](./postman-collection/Booking_Payment_API.postman_collection.json)** | Ready-to-import Postman v2.1 collection with automated JavaScript assertions (`pm.test`). |

---

## Key Testing Concepts Covered
* **HTTP Methods & Status Codes:** `200 OK`, `400 Bad Request`, `403 Forbidden`, `404 Not Found`.
* **Authentication & Security:** Token generation (`POST /auth`) and unauthorized modification blocking.
* **Data & Financial Integrity:** Validating payment amounts, boolean deposit flags, and negative-value edge cases.
