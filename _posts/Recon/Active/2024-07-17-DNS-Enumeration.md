---
layout: post
title: "DNS"
date: 2024-12-22
categories: Recon/Active
---

### Table of Contents

1. [Introduction to DNS](#introduction-to-dns)  
    1. [Overview](#overview)  
    2. [DNS Records](#dns-records)  
    3. [Common DNS Server Types](#common-dns-server-types)  
2. [Enumeration Techniques](#enumeration-techniques)  
    1. [Querying DNS Records](#querying-dns-records)  
    2. [Performing Zone Transfers](#performing-zone-transfers)  
3. [Advanced Techniques](#advanced-techniques)  
    1. [Subdomain Bruteforcing](#subdomain-bruteforcing)  
    2. [Discovering Hidden Services](#discovering-hidden-services)  
4. [Exploitation Scenarios](#exploitation-scenarios)  
    1. [Misconfigured DNS Servers](#misconfigured-dns-servers)  
    2. [DNS Spoofing and Cache Poisoning](#dns-spoofing-and-cache-poisoning)  
5. [Combining Tools](#combining-tools)  
    1. [DNS Enumeration with `dig`](#dns-enumeration-with-dig)  
    2. [Automating DNS Recon with `dnsenum`](#automating-dns-recon-with-dnsenum)  
6. [Glossary](#glossary)

---

## Introduction to DNS

### Overview

The Domain Name System (DNS) is the backbone of the internet, translating human-readable domain names (e.g., `example.com`) into machine-readable IP addresses (e.g., `192.168.1.1`).

**Core Concepts**:

- DNS resolves domain names to IP addresses using a distributed hierarchy of name servers.
- Communication is typically unencrypted, which makes it susceptible to attacks like eavesdropping and spoofing.

---

### DNS Records

| **Record Type** | **Description**                                                                 |
|------------------|---------------------------------------------------------------------------------|
| **A**           | Maps a domain to an IPv4 address.                                               |
| **AAAA**        | Maps a domain to an IPv6 address.                                               |
| **MX**          | Specifies mail servers for the domain.                                          |
| **NS**          | Indicates the authoritative name servers for a domain.                         |
| **CNAME**       | Creates an alias for a domain.                                                 |
| **PTR**         | Used for reverse DNS lookups (IP to domain).                                    |
| **TXT**         | Stores arbitrary text, often used for SPF, DMARC, or domain validation.         |
| **SOA**         | Contains administrative information about a zone, including the responsible email address. |

---

### Common DNS Server Types

| **Type**              | **Description**                                                                                     |
|-----------------------|-----------------------------------------------------------------------------------------------------|
| **Root Servers**      | Handle top-level domains like `.com` and `.org`.                                                   |
| **Authoritative NS**  | Provide definitive answers for specific zones.                                                     |
| **Caching DNS**       | Temporarily store query results to improve performance.                                            |
| **Forwarding DNS**    | Forward queries to another server.                                                                 |
| **Resolvers**         | Translate domain names into IP addresses locally (e.g., on a router or client).                   |

---

## Enumeration Techniques

### Querying DNS Records

Use tools like `dig` and `host` to query specific DNS records.

**Query SOA Record**:

```bash
dig soa example.com
```

**Query NS Record**:

```bash
dig ns example.com
```

**Query All Available Records**:

```bash
dig any example.com
```

**Example Output**:

```
example.com.     3600 IN  A     93.184.216.34
example.com.     3600 IN  NS    ns1.example.com.
example.com.     3600 IN  MX    10 mail.example.com.
```

---

### Performing Zone Transfers

Zone transfers (AXFR) replicate DNS zones between servers. Misconfigured DNS servers may allow anyone to request a full zone transfer.

**Command**:

```bash
dig axfr example.com @dns-server-ip
```

**Example Output**:

```
example.com.     IN  A       93.184.216.34
mail.example.com. IN  MX      10 93.184.216.35
```

---

## Advanced Techniques

### Subdomain Bruteforcing

Identify subdomains using a wordlist and DNS queries.

**Bash Loop**:

```bash
for sub in $(cat subdomains.txt); do
    dig +short $sub.example.com;
done
```

**Tools**:

- `dnsenum`
- `Sublist3r`

---

### Discovering Hidden Services

Look for services using non-standard DNS records like `TXT` or custom CNAMEs.

**Query TXT Records**:

```bash
dig txt example.com
```

---

## Exploitation Scenarios

### Misconfigured DNS Servers

Servers with improper configurations can leak sensitive information:

- **Open Zone Transfers**: Allow unauthorized AXFR queries.
- **Wildcard Records**: Misconfigured `*` records can lead to unintended access.

---

### DNS Spoofing and Cache Poisoning

Attackers can exploit unencrypted DNS traffic to redirect users to malicious domains.

**Prevention**:

- Use DNSSEC to verify responses.
- Encrypt traffic with DNS-over-HTTPS (DoH) or DNS-over-TLS (DoT).

---

## Combining Tools

### DNS Enumeration with `dig`

**Query Specific Record Types**:

```bash
dig a example.com
```

**Query Multiple Records**:

```bash
dig ns example.com +short
dig mx example.com +short
```

---

### Automating DNS Recon with `dnsenum`

**Command**:

```bash
dnsenum --enum -p 0 -s 0 -o output.txt -f subdomains.txt example.com
```

**Example Output**:

```
example.com:
- ns1.example.com
- mail.example.com
- api.example.com
```

---

## Glossary

| **Term**            | **Definition**                                                |
|----------------------|--------------------------------------------------------------|
| **AXFR**            | Asynchronous Full Transfer of Zone data between DNS servers. |
| **DNSSEC**          | DNS Security Extensions for validating DNS responses.        |
| **SOA**             | Start of Authority record for zone metadata.                 |
| **Wildcard Record** | DNS record that resolves all subdomains to a single address. |

---
