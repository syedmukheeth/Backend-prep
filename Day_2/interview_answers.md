# Day 2: HTTP/HTTPS Interview Answers

## Beginner Questions

### 1. What is the difference between `GET` and `POST`?
*   **Purpose**: `GET` is for retrieving data (Read); `POST` is for sending/submitting data (Create).
*   **Data Location**: `GET` sends data in the URL query string; `POST` sends data in the request body.
*   **Security**: `GET` is less secure for sensitive data (visible in URL/logs); `POST` is preferred for passwords/PII.
*   **Idempotency**: `GET` is idempotent (multiple calls don't change state); `POST` is not (multiple calls create multiple resources).
*   **Caching**: `GET` requests can be cached and bookmarked; `POST` requests cannot.

### 2. Name the 4 main families of HTTP Status Codes.
*   **2xx (Success)**: Everything worked (e.g., `200 OK`, `201 Created`).
*   **3xx (Redirection)**: Further action is needed to fulfill the request (e.g., `301 Moved Permanently`).
*   **4xx (Client Error)**: The client made a mistake (e.g., `400 Bad Request`, `404 Not Found`).
*   **5xx (Server Error)**: The server failed to process a valid request (e.g., `500 Internal Server Error`).

---

## Intermediate Questions

### 1. What is the difference between `PUT` and `PATCH`?
*   **PUT (Full Update)**: Replaces the **entire** resource. If you send a `PUT` request with only one field, the other fields on the server might be wiped or set to default. It is **idempotent**.
*   **PATCH (Partial Update)**: Only updates the specific fields provided in the request body. It is more efficient for small updates and is generally **not idempotent** by strict definition (though often implemented as such).

### 2. What does it mean that HTTP is stateless?
It means the server does not store any "memory" of previous requests. Every request is treated as a completely new transaction. 
*   **Impact**: To maintain a "session" (like staying logged in), the client must send credentials (like a JWT or Cookie) with **every single request**. The server doesn't "remember" you were logged in from the last request.

---

## Senior Questions

### 1. Explain the concept of Idempotency in REST APIs. Which methods are idempotent?
**Definition**: An operation is idempotent if performing it multiple times has the exactly same effect on the server's state as performing it once.
*   **Idempotent Methods**: 
    *   `GET`, `HEAD`, `OPTIONS`: Read operations never change state.
    *   `PUT`: Replacing a file with the same content 10 times results in the same file.
    *   `DELETE`: Deleting a resource that is already gone results in it still being gone.
*   **Non-Idempotent Methods**:
    *   `POST`: Calling "Create User" 5 times creates 5 users.
    *   `PATCH`: If an operation is "add $10 to balance", calling it 5 times adds $50.

### 2. Explain the TLS handshake step-by-step. How does HTTPS prevent Man-in-the-Middle attacks?
#### The Handshake Process:
1.  **Client Hello**: Client sends supported TLS versions and cipher suites.
2.  **Server Hello**: Server chooses the best encryption and sends its **SSL Certificate** (which includes its Public Key).
3.  **Authentication**: Client verifies the certificate with a trusted **Certificate Authority (CA)** to ensure the server is who it claims to be.
4.  **Key Exchange**: Client generates a random **Pre-master Secret**, encrypts it with the **Server's Public Key**, and sends it back.
5.  **Session Keys**: Only the server can decrypt this secret using its **Private Key**. Both now use this secret to generate a **Symmetric Session Key** for the rest of the conversation.

#### MITM Prevention:
HTTPS prevents MITM primarily through **Certificates**. An attacker might intercept the traffic, but they cannot provide a valid certificate for `google.com` that is signed by a trusted CA. If they try to "mimic" the server, the browser will see a certificate mismatch and block the connection. Furthermore, because the session key is encrypted with the server's public key, only the legitimate owner of the private key can access the data.
