## Lab 05: Web App Enumeration & HTTP

---

## 🧰 Requirements

- VMware Workstation (v15.5.7 or higher)
- Internet access
- Access to `http://FOLusername-uws/mutillidae`
- Kali Linux with:
  - Firefox
  - Burp Suite
- Firefox Add-on: User-Agent Switcher and Manager by Ray (or similar)

---

## Part 01: Capture HTTP Requests and Responses

### 🔹 Slide 01: Capture HTTP Request Showing User-Agent

**Steps:**
1. Open Firefox in Kali Linux.
2. Launch Burp Suite and ensure Firefox is using Burp’s proxy (127.0.0.1:8080).
3. Visit any Mutillidae page.
4. In Burp, go to HTTP History.
5. Select a GET or POST request.
6. In the **Raw tab**, find the `User-Agent` field.
7. Take a screenshot highlighting the `User-Agent`.

---

### 🔹 Slide 02: Capture HTTP Response with 404 Status Code

**Steps:**
1. In Firefox, navigate to a non-existent page on the Mutillidae server (e.g., `index.php?page=notfound.php`).
2. In Burp, find the response with a `404` status code.
3. Go to the **Raw response tab**.
4. Highlight the `404` status code and take a screenshot.

---

## Part 01 Continued: Session Hijacking

### 🔹 Slide 03: Capture Set-Cookie for First User

**Steps:**
1. Create user `FOLusername-01` in Mutillidae.
2. Log in as this user.
3. In Burp, find the login request and locate the `Set-Cookie` header in the response.
4. Highlight the `Set-Cookie` field and take a screenshot.

---

### 🔹 Slides 04 & 05: Hijack Session by Editing Cookie

**Steps:**
1. Create a second user: `FOLusername-02`.
2. Log in as `FOLusername-02`.
3. In Burp, intercept a request to "Add to Your Blog".
4. In the intercepted request, replace the session cookie with the one from `FOLusername-01`.
5. Forward the request.
6. Verify that you are now logged in as `FOLusername-01`.

- **Slide 04**: Take a screenshot of the **original request**.
- **Slide 05**: Take a screenshot of the **edited request**.

---

### 🔹 Slide 06: Hijack Admin Session

**Steps:**
1. Try to guess or identify the admin session ID.
2. Intercept a request while logged in.
3. Modify the cookie field to use the admin’s session ID.
4. Forward the request to access admin-only areas.
5. Take a screenshot of the edited request that attempts Admin access.

---

## Part 02: Modify User-Agent Header (JavaScript Injection)

### 🔹 Inject JavaScript into User-Agent via Burp

**Steps:**
1. Reset Mutillidae DB from the home page.
2. Visit a page like `index.php?page=credits.php`.
3. In Burp, intercept the request.
4. Modify the `User-Agent` header to:
   ```html
   <script type="text/javascript">alert("FOLusername");</script>
   ```
5. Forward the request and verify an alert box appears in Firefox.

---

### 🔹 Automate with User-Agent Switcher Add-On

**Steps:**
1. Install the "User-Agent Switcher and Manager" add-on in Firefox.
2. Create a new Mutillidae user: `FOLusername`.
3. Log in and go to the extension settings.
4. Customize the User-Agent string to:
   ```html
   <script>alert(Date() + "; " + document.cookie);</script>
   ```
5. Navigate within Mutillidae to trigger the header injection.
6. In Burp, capture the request.
7. Verify the alert box shows date/time and cookie info.

- **Slide 07**: Take a screenshot showing:
  - The original POST request
  - The alert popup with cookie and date info

---

## Part 03: Burp Suite Intruder – Brute Force Login

### 🔹 Slide 08: Brute Force with Burp Intruder

**Steps:**
1. Create user `folusername` with password `foobar`.
2. Visit the login page.
3. Enter `folusername` and a random wrong password.
4. In Burp, intercept the login request.
5. Right-click > Send to Intruder.
6. In Intruder > Positions tab:
   - Keep only the `password` field as a variable.
7. In Payloads tab:
   - Generate 15 passwords from [random.org/passwords](https://random.org/passwords).
   - Add `foobar` to the list.
   - Paste into Burp’s payload list.
8. Click "Start attack".
9. Look for a response with `302` status (successful login).
10. Take a screenshot of the raw request that caused the redirect.

---

## 📸 Final Task

- Take VM snapshots labeled: **Lab 05**
- Include all major tools running (Firefox, Burp Suite, Mutillidae)
