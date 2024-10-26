# HTTP vs HTTPS Mind Map

## 1. Introduction
- **HTTP (Hypertext Transfer Protocol)**: A protocol for sending data between a web browser and a website.
- **HTTPS (Hypertext Transfer Protocol Secure)**: An encrypted version of HTTP for secure communication.

---

## 2. Core Difference
- **HTTP**: Data is sent as plain text, vulnerable to interception.
- **HTTPS**: Uses encryption to protect data from being intercepted by third parties.

---

## 3. Security Mechanism
- **HTTP**: No encryption; anyone eavesdropping on the network can read the data.
- **HTTPS**:
  - Uses **SSL/TLS** encryption.
  - Prevents tampering and protects sensitive data.

---

## 4. Data Encryption
- **HTTP**: No encryption, making data easily readable.
- **HTTPS**: Encrypts data so that only the intended recipient can decrypt it.

---

## 5. Authentication
- **HTTP**: No verification of the server’s identity.
- **HTTPS**:
  - Uses **Certificates** from trusted authorities (CA) to verify the authenticity of the website.
  - Prevents **man-in-the-middle attacks**.

---

## 6. Performance
- **HTTP**: Faster because it skips encryption and decryption processes.
- **HTTPS**: Slightly slower due to the overhead of encryption, but modern improvements (e.g., HTTP/2) mitigate this.

---

## 7. Browser Indicators
- **HTTP**: Marked as **“Not Secure”** in modern browsers.
- **HTTPS**: Shows a **padlock icon** in the browser’s address bar, indicating the connection is secure.

---

## 8. SEO and Trust
- **HTTP**: Websites using only HTTP may rank lower in search engines.
- **HTTPS**:
  - **SEO Boost**: Search engines like Google prioritize HTTPS websites.
  - Builds **trust** with users by securing transactions and protecting personal information.

---

## 9. Use Cases
- **HTTP**: Suitable for non-sensitive data (e.g., blogs, informational websites).
- **HTTPS**: Required for websites that handle sensitive data (e.g., banking, online shopping).

---

## 10. Transition from HTTP to HTTPS
- **TLS/SSL Certificate**: Websites need to install a certificate from a trusted **Certificate Authority (CA)**.
- **Redirects**: HTTP traffic should be redirected to the HTTPS version to ensure secure communication.

---

## 11. Conclusion
- **HTTPS**: Now the standard for secure web communication, ensuring privacy and trust.
- **HTTP**: Still used, but increasingly replaced due to security concerns.