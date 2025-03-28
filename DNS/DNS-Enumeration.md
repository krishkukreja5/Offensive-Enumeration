# DNS Enumeration

## DNS Enumeration
DNS Enumeration is the process of collecting information about a domain and its DNS records to map out the domain’s infrastructure. It helps identify subdomains, mail servers, name servers, and other publicly available information that could reveal potential attack vectors.

## Vertical Domain Co-Relation
Examples of vertical domain co-relation, where subdomains are associated with the main domain:

- **armourinfosec.com**  
  ```bash
  admin.armourinfosec.com  
  mail.armourinfosec.com  
  ```

- **tesla.com**  
  ```bash
  admin.tesla.com  
  mail.tesla.com  
  cp.tesla.com  
  test.tesla.com  
  ```

## Horizontal Domain Co-Relation
Examples of horizontal domain co-relation, where domains are associated across different TLDs or organizations:

- **Armourinfosec domains**  
  ```bash
  *.thehackersworld.com  
  *.infosecwarrior.com  
  *.armourinfosec.io  
  ```

- **Tesla domains**  
  ```bash
  *.tesla.cn  
  *.teslamotors.com  
  *.tesla.services  
  *.teslainsuranceservices.com  
  *.solarcity.com  
  ```

## Why DNS Enumeration is Important

- **Information Gathering** – It allows attackers and security testers to gather data about the target's network.  

- **Identifying Attack Surfaces** – Misconfigured DNS records, exposed subdomains, and outdated infrastructure can be potential entry points.  

- **Social Engineering** – Collected data can be used to craft more targeted phishing or social engineering attacks.  

- **Pentesting & Red Teaming** – Understanding the DNS infrastructure helps in simulating real-world attack scenarios.

## Types of DNS Enumeration  

1. **Direct Domain Enumeration**  
   - Identifying subdomains and associated records through direct queries.  

2. **Reverse DNS Lookup**  
   - Resolving an IP address back to its domain name.  

3. **Zone Transfer (AXFR) Enumeration**  
   - Attempting to transfer the entire DNS zone file from a misconfigured DNS server.  

4. **Brute-Force Subdomain Discovery**  
   - Trying common subdomain names to identify valid ones.  

5. **PTR Record Lookup**  
   - Resolving IP addresses to domain names.  

6. **Certificate Transparency Logs**  
   - Checking for subdomains and domains listed in publicly available SSL/TLS certificates.  

7. **WHOIS Lookup**  
   - Gathering domain registration and ownership details.  

## Target Range

IP Address to Domain Name mapping within a specified range:

- 192.168.1.1/24
- 192.31.242.107
- 192.31.242.107/32
- 2620:134:b000::/40
- 192.31.243.0/26
- 192.31.243.64/28
- 192.31.243.96/27

## Common DNS Record Types

| Record Type | Description                                             |
|-------------|----------------------------------------------------------|
| A           | Maps a domain to an IPv4 address                         |
| AAAA        | Maps a domain to an IPv6 address                         |
| CNAME       | Alias of one domain to another                           |
| MX          | Mail server records                                      |
| NS          | Name server records                                      |
| SOA         | Start of Authority, contains domain admin and refresh info|
| TXT         | Text records, used for SPF, DKIM, and other metadata     |
| PTR         | Maps an IP address to a domain (reverse lookup)          |

## Key DNS Enumeration Tools

| Tool       | Purpose                             | Example Command                             |
|------------|-------------------------------------|----------------------------------------------|
| nslookup   | Query DNS records                    | nslookup armourinfosec.com                   |
| dig        | Detailed DNS lookup                  | dig armourinfosec.com ANY                    |
| host       | Simple DNS lookup                    | host armourinfosec.com                       |
| nmap       | DNS and port scanning                | nmap -p 53 --script=dns-brute armourinfosec.com|
| dnsrecon   | Automated DNS enumeration            | dnsrecon -d armourinfosec.com                |
| dnsenum    | Brute force DNS enumeration          | dnsenum armourinfosec.com                    |
| sublist3r  | Subdomain enumeration                | sublist3r -d armourinfosec.com               |
| crt.sh     | Certificate search for subdomains    | https://crt.sh/?q=armourinfosec.com          |

## Example DNS Enumeration Commands

1. Find all DNS records using dig:
   ```bash
   dig armourinfosec.com ANY
   ```

2. Reverse lookup for PTR records using dig:
   ```bash
   dig -x 192.168.1.43
   ```

3. Find subdomains using dnsrecon:
   ```bash
   dnsrecon -d armourinfosec.com -t std
   ```

4. Find subdomains using sublist3r:
   ```bash
   sublist3r -d armourinfosec.com
   ```

5. Perform zone transfer using dig:
   ```bash
   dig axfr @ns1.armourinfosec.com armourinfosec.com
   ```

6. Identify DNS servers using nslookup:
   ```bash
   nslookup -type=NS armourinfosec.com
   ```

7. Enumerate DNS records with nmap:
   ```bash
   nmap -p 53 --script dns-brute armourinfosec.com
   ```

## Port Scan for Domain Name Enumeration with nmap
Perform a port scan to identify open DNS ports:

```bash
nmap -v -Pn -sT -sU -sV -p T:53,U:53 -iL ip.txt
```

- -v       : - Verbose mode
- -Pn      : - Treat all hosts as online
- -sT      : - TCP connect scan
- -sU      : - UDP scan
- -sV      : - Version detection
- -p T:53,U:53 : - Scan TCP and UDP port 53 (DNS)
- -iL ip.txt : -   Input list of IP addresses

## Check SSL Certificate
You can check the SSL certificate using the following OpenSSL command:

```bash
openssl s_client -connect domain.com:443
```

## Follow HTTP Redirection
To follow HTTP and HTTPS redirection:

```bash
curl -I -L http://domain.com
```

- -I :- Show headers only
- -L :- Follow redirects

## Find PTR Records of IP Address / Find Internal DNS Server
To find PTR records and identify internal DNS servers:

1. Using nmap:
   ```bash
   nmap -v -sV -sC -A -O -p 80,443 192.168.1.43 --dns-servers 192.168.1.43
   ```

2. Using nslookup:
   ```bash
   nslookup 192.168.1.43 192.168.1.43
   ```

3. Using dig:
   ```bash
   dig -x 192.168.1.43 @192.168.1.43
   ```

4. Using dnsx:
   ```bash
   dnsx -silent -resp-only -ptr -l /home/armour/Documents/ip.txt
   dnsx -silent -resp-nly -ptr -l /tmp/ip.txt
   ```

5. Find ASN (Autonomous System Number) using nmap:
   ```bash
   nmap --script targets-asn --script-args targets-asn.asn=394161
   ```

6. Query domain using nslookup:
   ```bash
   nslookup ipsacademy.org
   ```

## IP Address History
You can find the historical IP address records of a domain using the following online services:

- https://viewdns.info/iphistory/
- Example: https://viewdns.info/iphistory/?domain=armourinfosec.com

## Common Google Subdomains
Examples of commonly used Google subdomains:

- myaccount.google.com
- maps.google.co.in
- play.google.com
- news.google.com
- mail.google.com
- meet.google.com
- chat.google.com

## Common Facebook Subdomains
Examples of commonly used Facebook subdomains:

- www.facebook.com
- m.facebook.com
- developers.facebook.com
- portal.facebook.com
- play.facebook.com

## Common GoDaddy Subdomains
Examples of commonly used GoDaddy subdomains:

- in.godaddy.com
- kr.godaddy.com
- pe.godaddy.com
- pl.godaddy.com
- fr.godaddy.com

## Common Government Subdomains
Examples of commonly used Indian government subdomains:

- www.incredibleindia.org
- www.india.gov.in
- services.india.gov.in
- rti.india.gov.in
- covid19.india.gov.in

## Public DNS Servers
Common public DNS servers:

```bash
- 8.8.8.8       # Google DNS
- 8.8.4.4       # Google DNS
- 64.6.64.6     # Verisign DNS
```




