## Troubleshooting  

During my setup and testing of **GoPhish**, I encountered issues that required investigation. Below are the key problems and resolutions:  

### 1. Installation Errors  
While installing GoPhish, I had a few initial configuration issues that required adjusting paths and ensuring dependencies were correctly set up.  

---

### 2. Problem: GoPhish keeps restarting or using a new password every time you run it  

**Solution:**  

Check for running processes:  
```bash
sudo netstat -tulnp | grep LISTEN

```bash
sudo ss -tulnp | grep :80
````

Kill the GoPhish process safely (no need to disable the service unless permanently stopping):

Find the PID and process tree:

```bash
pstree -p <PID>
```

Stop the GoPhish service:

```bash
sudo systemctl status gophish
sudo systemctl stop gophish
```

Rerun the executable:

```bash
./gophish
```

This will print the default admin credentials again.

---

### 3. Landing Page Not Collecting Credentials

**Problem:** The landing page was not capturing credential submissions.

**Root Cause Analysis:**

* GoPhish does **not** support JavaScript execution for credential capture. Any attempt to use JavaScript-based forms will fail.
* The `<form>` element must be properly defined. If the format is incorrect, GoPhish cannot parse the submission.
* Using an external `action="..."` attribute in the form breaks the capture process, as the data bypasses GoPhish entirely.

**Resolution:**
Use either an empty `<form>` tag or explicitly define the method as `POST`, without pointing to an external script:

```html
<form method="POST">
    <input type="text" name="username" placeholder="Phone number, username or Email" required>
    <input type="password" name="password" placeholder="Password" required>
    <button type="submit">Sign In</button>
</form>
```

```

---


