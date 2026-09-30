# Swagger Petstore — Endpoint Coverage Matrix

## API Overview

| Field | Value |
|-------|-------|
| **Spec Version** | OpenAPI 2.0 (Swagger) |
| **Base URL** | `https://petstore.swagger.io/v2` |
| **Total Endpoints** | 20 |
| **Tags (Groups)** | 3 — Pet, Store, User |
| **Models** | 6 — Pet, Category, Tag, Order, User, ApiResponse |
| **Auth** | API Key (header) + OAuth2 (implicit flow) |

---

## Endpoint Coverage

### Pet Endpoints (8)

| # | Method | Endpoint | Summary | Tested in Postman | Manual Test Case |
|---|--------|----------|---------|:-----------------:|:----------------:|
| 1 | POST | `/pet` | Add a new pet | Yes | TC-01 |
| 2 | PUT | `/pet` | Update an existing pet | Yes | TC-02 |
| 3 | GET | `/pet/findByStatus` | Find pets by status | Yes | TC-03 |
| 4 | GET | `/pet/findByTags` | Find pets by tags (deprecated) | Yes | TC-04 |
| 5 | GET | `/pet/{petId}` | Find pet by ID | Yes | TC-05 |
| 6 | POST | `/pet/{petId}` | Update pet with form data | Yes | TC-06 |
| 7 | DELETE | `/pet/{petId}` | Delete a pet | Yes | TC-07 |
| 8 | POST | `/pet/{petId}/uploadImage` | Upload pet image | Yes | TC-08 |

### Store Endpoints (4)

| # | Method | Endpoint | Summary | Tested in Postman | Manual Test Case |
|---|--------|----------|---------|:-----------------:|:----------------:|
| 9 | GET | `/store/inventory` | Get inventory by status | Yes | TC-09 |
| 10 | POST | `/store/order` | Place an order | Yes | TC-10 |
| 11 | GET | `/store/order/{orderId}` | Find order by ID | Yes | TC-11 |
| 12 | DELETE | `/store/order/{orderId}` | Delete order by ID | Yes | TC-12 |

### User Endpoints (8)

| # | Method | Endpoint | Summary | Tested in Postman | Manual Test Case |
|---|--------|----------|---------|:-----------------:|:----------------:|
| 13 | POST | `/user` | Create a user | Yes | TC-13 |
| 14 | POST | `/user/createWithArray` | Create users (array) | Yes | TC-14 |
| 15 | POST | `/user/createWithList` | Create users (list) | Yes | TC-15 |
| 16 | GET | `/user/login` | Login | Yes | TC-16 |
| 17 | GET | `/user/logout` | Logout | Yes | TC-17 |
| 18 | GET | `/user/{username}` | Get user by username | Yes | TC-18 |
| 19 | PUT | `/user/{username}` | Update user | Yes | TC-19 |
| 20 | DELETE | `/user/{username}` | Delete user | Yes | TC-20 |

---

## Coverage Summary

| Tag | Endpoints | Covered | Coverage |
|-----|-----------|---------|----------|
| Pet | 8 | 8 | 100% |
| Store | 4 | 4 | 100% |
| User | 8 | 8 | 100% |
| **Total** | **20** | **20** | **100%** |

---

## Data Models

| Model | Key Fields | Used By |
|-------|-----------|---------|
| **Pet** | id, name (required), photoUrls (required), status (available/pending/sold), category, tags | POST/PUT/GET /pet |
| **Category** | id, name | Nested in Pet |
| **Tag** | id, name | Nested in Pet |
| **Order** | id, petId, quantity, shipDate, status (placed/approved/delivered), complete | POST/GET /store/order |
| **User** | id, username, firstName, lastName, email, password, phone, userStatus | POST/GET/PUT /user |
| **ApiResponse** | code, type, message | Upload image response |

---

## Observations & Spec Issues Found

1. **Deprecated endpoint still active**: `GET /pet/findByTags` is marked deprecated in the spec but still returns data — no sunset header or error response
2. **No pagination**: `findByStatus` and `findByTags` return unbounded arrays — no `limit`/`offset` parameters
3. **Order ID range restriction**: `GET /store/order/{orderId}` only accepts IDs 1–10 per the spec, but this isn't enforced consistently
4. **Password in query string**: `GET /user/login` passes credentials as query parameters (`?username=x&password=y`) — a security concern even for a demo API
5. **Missing validation**: POST `/pet` accepts a pet without required `photoUrls` field in practice (returns 200 instead of 400)
6. **Inconsistent error responses**: Some endpoints return structured `ApiResponse` JSON on error, others return plain text or HTML

---

## Author

**Jigyasa Gupta** — QA Engineer
[GitHub](https://github.com/jgupta-git)
