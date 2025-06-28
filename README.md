## Namaste! 

![Logo](https://github.com/user-attachments/assets/259086bb-2022-4653-87e3-923ec1b2e9c9)

![Profile_Views](https://komarev.com/ghpvc/?username=HacktivistRO&style=for-the-badge)

# Bug-Bounty-Recon
This workflow shows how to probe deeper and uncover rewards others overlook.

**Note:** The tool commands shown here are subject to change. Always refer to the official documentation for the latest usage and syntax.

## 1. Confirm Scope

- Verify every target domain (for example, `*.hacktivistro.com`)
- Ignore anything marked out of scope to stay efficient and legal

---

## 2. Enumerate Subdomains

**Tools**

- [`SubSleuth`](https://github.com/HacktivistRO/SubSleuth)
- [`bbot`](https://github.com/blacklanternsecurity/bbot)

```bash
sudo bash subsleuth.sh hacktivistro.com
bbot -t domain hacktivistro.com
cat *.txt | sort -u > subdomains.txt
```
Passive and active enumeration together yield the broadest inventory.

---

## 3. Identify Live Hosts

**Tool:** [`httpx`](https://github.com/projectdiscovery/httpx)

```bash
cat subdomains.txt | httpx -silent -o alive.txt
```
Only scan hosts that respond; everything else is noise.

---

## 4. Crawl for Hidden Content

**Tool:** [`katana`](https://github.com/projectdiscovery/katana)

```bash
katana -list alive.txt -o endpoints.txt
```

Crawler output reveals directories, undocumented APIs, and buried endpoints.

---

## 5. Capture Screenshots

**Tool:** [`EyeWitness`](https://github.com/FortyNorthSecurity/EyeWitness)

```bash
eyewitness --web -f alive.txt --threads 10 -d screenshots
```

Visual reconnaissance accelerates triage and enables the identification of unusual interfaces quickly.

---

## 6. Automate Vulnerability Discovery

**Tools**

- [`nuclei`](https://github.com/projectdiscovery/nuclei)
- [`nmap`](https://nmap.org/)
- [`nikto`](https://github.com/sullo/nikto)

```bash
cat alive.txt | nuclei -t templates/ -o nuclei.txt
nmap -sVC -T4 -iL alive.txt -oN nmap.txt
nikto -h alive.txt -output nikto.txt
```
Leverage templates and scanners for misconfigurations, outdated software, and default credentials.

---

## 7. Detect the Tech Stack
**Tools**

- [Wappalyzer](https://www.wappalyzer.com/)
- [BuiltWith](https://builtwith.com/)
- [WhatRuns](https://www.whatruns.com/)

Once the stack is known, map it to recent CVEs for fast prioritization.

---

## 8. Low‑Hanging Fruit

**Tools**

- [`HostileSubBruteForcer by HacktivistRO`](https://github.com/HacktivistRO/HostileSubBruteForcer)
- [`subzy`](https://github.com/LukaSikic/subzy)

```bash
ruby sub_brute.rb
subzy run --targets alive.txt
```

Quickly spot subdomain takeover opportunities and broken social links.

---

## 9. Recover Archived URLs and Parameters

**Tools**

- [`waybackurls`](https://github.com/tomnomnom/waybackurls)
- [`gau`](https://github.com/lc/gau)
- [`paramspider`](https://github.com/devanshbatham/paramspider)

```bash
cat alive.txt | waybackurls >> urls.txt
cat alive.txt | gau >> urls.txt
paramspider -d hacktivistro.com -o params.txt
```

Historical endpoints often reveal previously overlooked functionality and inadequate validation.

---

## 10. Craft Search Dorks
**Resources**
- [`Interesting Google Dorks`](https://github.com/HacktivistRO/Bug-Bounty-Wordlists/tree/master/Wordlists#:~:text=2%20years%20ago-,google%2Ddorks%2Dfor%2Dsecrets.txt,-Google%20Dorks%20for)

```text
site:hacktivistro.com ext:sql
site:hacktivistro.com inurl:admin
site:hacktivistro.com ext:bak
```

Tailored search queries uncover exposed panels, database exports, and backups.

---

## 11. Mine GitHub for Secrets
**Resources**
- [`Interesting GitHub Dorks`](https://github.com/HacktivistRO/Bug-Bounty-Wordlists/tree/master/Wordlists#:~:text=2%20years%20ago-,github%2Ddorks%2Dfor%2Dsecrets.txt,-Wordlist%20for%20GitHub)

```text
"hacktivistro.com" in:code
```

Code searches reveal credentials, API keys, and internal hosts committed by mistake.

---

## Bonus: Quick Parameter Exploits

**Tools**

- [`gf`](https://github.com/tomnomnom/gf)
- [`qsreplace`](https://github.com/tomnomnom/qsreplace)
- [`httpx`](https://github.com/projectdiscovery/httpx)

```bash
gf xss urls.txt | qsreplace '"><script>alert(1)</script>' | httpx -silent
```

Pipe parameters into payloads for rapid XSS, SQLi, and LFI checks at scale.

---

## Pro Hunter Principles

- Automate repetitive tasks  
- Investigate areas others forget  
- Track what you learn; iterate and refine  

Real recon brings real rewards—let the crowd stay noisy while you remain effective.

**Next Steps:** Feeling stuck of what to do once recon is done? Follow [this guide](https://github.com/HacktivistRO/Bug-Bounty-Wordlists/blob/master/how-to-start-bug-hunting.txt) for such situations.

---

<p align="center">
Happy Hacking! :heart:
</p> 
