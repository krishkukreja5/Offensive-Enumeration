# VHOST Fuzzing with Wfuzz
## What is Wfuzz?
Wfuzz is a powerful and flexible web application brute-forcing tool, used primarily for:

Discovering hidden resources (like subdomains, directories, and files).

Fuzzing or brute-forcing parameters, URLs, headers, cookies, and more.

Analyzing responses to identify valid vs. error or redirect responses.

## VHOST Fuzzing
Unlike traditional fuzzing that targets URLs, VHOST fuzzing targets the Host header to discover virtual hosts configured on a web server.

**Scenario:**
We're querying subdomains via the Host header rather than directly requesting a URL.

# Example Use Case
Let’s say we are testing tesla.com for virtual hosts:

```bash
wfuzz -c \
-W /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt \
-u https://www.tesla.com \
-H "Host: FUZZ.tesla.com" \
--hh 5539 \
-t 50
``` 

## Explanation:

**-c** → color output

**-W** → wordlist for subdomain fuzzing

**-u** → base URL to send requests to

**-H** → custom Host header where FUZZ is replaced by each word

**--hh 5093** → hide responses with 5093 characters (likely false positives)

**-t 50** → 50 threads (faster execution)

# Filtering by HTTP Status Code
**To hide 301 redirects, use:**
```bash
wfuzz -c \
-W /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt \
-u https://www.tesla.com \
-H "Host: FUZZ.tesla.com" \
--hc 301 \
-t 50

```

# Other Useful Filters
### --hl <number> → hide by line count

### --hw <number> → hide by word count

### --hh <number> → hide by character count

### --hc <code> → hide by HTTP status code

# get help command
wfuzz --help
