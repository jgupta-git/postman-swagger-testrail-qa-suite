# Bug Reports — Petstore API Manual Testing

## BUG-001: Password exposed in query string on login endpoint

| Field | Detail |
|-------|--------|
| **ID** | BUG-001 |
| **Title** | Login endpoint passes credentials in URL query parameters |
| **Severity** | High |
| **Priority** | High |
| **Status** | Open |
| **Found In** | TC-16 (Login — valid credentials) |
| **Endpoint** | `GET /user/login?username={user}&password={pass}` |
| **Environment** | Petstore — Dev (`https://petstore.swagger.io/v2`) |

**Description:**
The login endpoint transmits username and password as query string parameters in a GET request. Query parameters are logged in server access logs, browser history, proxy logs, and may appear in referrer headers — exposing credentials.

**Steps to Reproduce:**
1. Open Postman
2. Send `GET https://petstore.swagger.io/v2/user/login?username=qaengineer&password=Test@1234`
3. Observe the password is visible in the URL

**Expected Result:**
Credentials should be sent in the request body (POST) or as an Authorization header — never as query parameters.

**Actual Result:**
Password is passed as a plain-text query parameter in a GET request.

**Impact:**
Credential leakage through server logs, browser history, and network intermediaries.

---

## BUG-002: Deprecated endpoint returns data with no deprecation warning

| Field | Detail |
|-------|--------|
| **ID** | BUG-002 |
| **Title** | `GET /pet/findByTags` is marked deprecated but returns 200 with no sunset header |
| **Severity** | Low |
| **Priority** | Low |
| **Status** | Open |
| **Found In** | TC-04 (Find pets by tags) |
| **Endpoint** | `GET /pet/findByTags?tags={tag}` |
| **Environment** | Petstore — Dev |

**Description:**
The OpenAPI spec marks `findByTags` as `deprecated: true`, but the endpoint still responds with `200 OK` and returns data. There is no `Sunset` header, no `Deprecation` header, and no warning in the response body to inform consumers.

**Steps to Reproduce:**
1. Send `GET https://petstore.swagger.io/v2/pet/findByTags?tags=golden-retriever`
2. Check response status and headers

**Expected Result:**
Either return a `410 Gone` or include a `Deprecation` / `Sunset` header with a removal date per [RFC 8594](https://www.rfc-editor.org/rfc/rfc8594).

**Actual Result:**
Status 200 with full data. No deprecation headers. Response is indistinguishable from a non-deprecated endpoint.

**Impact:**
API consumers have no runtime signal that this endpoint will be removed, leading to silent breakage when it is eventually shut down.

---

## BUG-003: Missing server-side validation for required fields on POST /pet

| Field | Detail |
|-------|--------|
| **ID** | BUG-003 |
| **Title** | `POST /pet` accepts payload missing required `name` field |
| **Severity** | Medium |
| **Priority** | Medium |
| **Status** | Open |
| **Found In** | TC-25 (Add pet without required field) |
| **Endpoint** | `POST /pet` |
| **Environment** | Petstore — Dev |

**Description:**
The OpenAPI spec defines `name` and `photoUrls` as required fields on the Pet model. However, sending a POST request without the `name` field returns `200 OK` instead of `400 Bad Request`.

**Steps to Reproduce:**
1. Send `POST https://petstore.swagger.io/v2/pet` with body:
   ```json
   {
     "id": 99999,
     "photoUrls": ["https://example.com/img.jpg"],
     "status": "available"
   }
   ```
2. Observe the response

**Expected Result:**
Status `400 Bad Request` with a validation error message indicating `name` is required.

**Actual Result:**
Status `200 OK`. Pet is created without a name.

**Impact:**
Data integrity issue — pets can be created with null/missing names, which may break downstream consumers expecting the field to be present.

---

## Summary

| Bug ID | Severity | Endpoint | Category |
|--------|----------|----------|----------|
| BUG-001 | High | `GET /user/login` | Security |
| BUG-002 | Low | `GET /pet/findByTags` | API Standards |
| BUG-003 | Medium | `POST /pet` | Validation |

---

## Author

**Jigyasa Gupta** — QA Engineer
[GitHub](https://github.com/jgupta-git)
