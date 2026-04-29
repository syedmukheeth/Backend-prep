# 🎯 Day 1: Interview Question Answers (Simplified)

This document contains the "lame terms" explanations for the key concepts covered in Day 1.

---

## 🟢 Beginner Level

### 1. IP Address vs. MAC Address
*   **MAC Address (Physical):** Like your **Fingerprint**. It is unique to your hardware and never changes. It identifies *who* you are.
*   **IP Address (Logical):** Like your **Hotel Room Number**. It changes depending on where you connect to the internet. It identifies *where* you are.

### 2. HTTP vs. HTTPS Ports
*   **HTTP (Unsecured):** Uses **Port 80**. (Like a glass door; everyone can see inside).
*   **HTTPS (Secured):** Uses **Port 443**. (Like an armored vault door; everything is encrypted).

---

## 🟡 Intermediate Level

### 3. TCP vs. UDP
*   **TCP (Transmission Control Protocol):** The **Reliable Courier**. It makes sure every single piece of data arrives perfectly and in order. If something is lost, it sends it again.
    *   *Use case:* Emails, Files, Web pages.
*   **UDP (User Datagram Protocol):** The **Fast Shooter**. It blasts data as fast as possible and doesn't care if some gets lost.
    *   *Use case:* Zoom calls, Online gaming, Live streaming.

### 4. Localhost / 127.0.0.1
*   It is the computer version of saying **"Myself."**
*   When you visit `localhost`, your computer talks to itself instead of sending data out to the internet. This is used for testing your code locally.

---

## 🔴 Senior Level

### 5. What happens when you type "google.com" and press Enter?
1.  **DNS Lookup:** Your browser finds the "phone number" (IP Address) for google.com using a global phonebook.
2.  **TCP Handshake:** Your computer and the server say "Hello" to each other to make sure the connection is ready.
3.  **TLS Handshake:** They exchange secret keys to make the connection private (HTTPS).
4.  **HTTP Request:** Your browser asks, "Can I have your home page code?"
5.  **Server Response:** Google's backend prepares the data and sends it back (Status 200 OK).
6.  **Rendering:** Your browser "draws" the website using the HTML, CSS, and JS it received.

---

## 👔 HR / Behavioral

### 6. "Tell me about a time an application went down..."
**Use the STAR Method:**
*   **Situation:** The site crashed during a big launch.
*   **Task:** I had to find the cause and fix it immediately.
*   **Action:** I checked the **Logs** and **Database Metrics**, found a missing index that was slowing everything down, and temporarily disabled the heavy feature.
*   **Result:** The site was back in 5 minutes, and I added a "Query Check" to our pipeline to prevent it from happening again.
