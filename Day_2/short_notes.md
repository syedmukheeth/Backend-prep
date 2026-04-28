# Day 2: Short Revision Notes

* **HTTP is Stateless**: Every request is independent. The server forgets you instantly. You must send auth info every time.
* **Idempotency**: Executing the request 1 time or 100 times has the exact same result. `GET`, `PUT`, `DELETE` are idempotent. `POST` is NOT.
* **Methods**:
    * `GET`: Read data. Never send sensitive info in the URL.
    * `POST`: Create new data. Not idempotent.
    * `PUT`: Update data by **replacing** the whole object.
    * `PATCH`: Update data by **modifying** partial fields.
    * `DELETE`: Remove data.
* **Status Codes**:
    * `2xx`: Success (`200 OK`, `201 Created`, `204 No Content`).
    * `3xx`: Redirects (`301 Moved Permanently`).
    * `4xx`: Client Errors - **Your fault** (`400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`).
    * `5xx`: Server Errors - **My fault** (`500 Internal Server Error`, `502 Bad Gateway`).
* **HTTPS/TLS**: Encrypts HTTP. Uses Asymmetric encryption for the handshake (Public/Private key) to securely exchange a symmetric "Session Key", which is then used for the fast, encrypted data transfer.
