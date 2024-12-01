---
layout: post
title: "Google Dorking"
date: 2024-11-30
categories: Recon/Passive
---

## **Table of Contents (ToC)**

1. [Basic Syntax Structure](#basic-syntax-structure)
2. [Common Google Dork Operators](#common-google-dork-operators)
    - [1. `site:` - Search within a specific website or domain](#1-site)
    - [2. `filetype:` - Search for specific file types](#2-filetype)
    - [3. `intitle:` - Search for a specific word or phrase in the title](#3-intitle)
    - [4. `inurl:` - Search for specific words in a URL](#4-inurl)
    - [5. `allinurl:` - Search for multiple words in a URL](#5-allinurl)
    - [6. `allintitle:` - Search for multiple words in the title](#6-allintitle)
    - [7. `intext:` - Search for specific text in the page body](#7-intext)
    - [8. `allintext:` - Search for multiple words in the page body](#8-allintext)
    - [9. `cache:` - View the cached version of a webpage](#9-cache)
    - [10. `ext:` - Search for files with specific extensions](#10-ext)
    - [11. `related:` - Find websites related to a given site](#11-related)
    - [12. `link:` - Find pages that link to a specific site](#12-link)
    - [13. `-` (Exclude Results) - Exclude specific words or operators](#13---exclude-results)
    - [14. `" "` (Exact Match) - Search for an exact phrase](#14--exact-match)
    - [15. `OR` - Combine two search conditions](#15-or)
    - [16. `*` (Wildcard) - Use a wildcard for unknown words/characters](#16--wildcard)
    - [17. Combining Multiple Operators - Build advanced queries](#17-combining-multiple-operators)
3. [General Tips - Logical grouping and experimentation](#general-tips)

---

### Basic Syntax Structure

The syntax for **Google Dorks** is based on using **advanced search operators** combined with specific keywords to refine search results. Here’s a breakdown of the syntax and how to use it:

---

### **Basic Syntax Structure**

```
[search operator]:[value] [keyword or text]
```

- **Search operator**: Defines the specific type of information you want to search for.
- **Value**: Specifies the condition or target for the operator (e.g., a file type, a website, or an inurl string).
- **Keyword or text**: Any additional keywords to refine the search.

---

### **Common Google Dork Operators**

Here’s a list of commonly used operators, their purpose, and examples:

---

#### **1. `site:`**

Search within a specific website or domain.

- **Syntax:**

  ```
  site:example.com
  ```

- **Example:**

  ```
  site:example.com filetype:pdf
  ```

  *(Find PDF files on example.com.)*

---

#### **2. `filetype:`**

Search for specific file types (e.g., PDFs, Excel sheets, SQL files).

- **Syntax:**

  ```
  filetype:[extension]
  ```

- **Example:**

  ```
  filetype:txt "password"
  ```

  *(Find `.txt` files containing the word “password.”)*

---

#### **3. `intitle:`**

Search for a specific word or phrase in the title of a page.

- **Syntax:**

  ```
  intitle:[word or phrase]
  ```

- **Example:**

  ```
  intitle:"index of"
  ```

  *(Find directory listings with "index of" in the title.)*

---

#### **4. `inurl:`**

Search for specific words in a URL.

- **Syntax:**

  ```
  inurl:[word or phrase]
  ```

- **Example:**

  ```
  inurl:admin
  ```

  *(Find URLs containing “admin.”)*

---

#### **5. `allinurl:`**

Search for multiple words in a URL.

- **Syntax:**

  ```
  allinurl:[words]
  ```

- **Example:**

  ```
  allinurl:login admin
  ```

  *(Find URLs containing both “login” and “admin.”)*

---

#### **6. `allintitle:`**

Search for multiple words in the title.

- **Syntax:**

  ```
  allintitle:[words]
  ```

- **Example:**

  ```
  allintitle:secure login
  ```

  *(Find pages with “secure” and “login” in the title.)*

---

#### **7. `intext:`**

Search for specific text within the body of a page.

- **Syntax:**

  ```
  intext:[word or phrase]
  ```

- **Example:**

  ```
  intext:"confidential"
  ```

  *(Find pages containing the word “confidential.”)*

---

#### **8. `allintext:`**

Search for multiple words in the text of a page.

- **Syntax:**

  ```
  allintext:[words]
  ```

- **Example:**

  ```
  allintext:username password
  ```

  *(Find pages containing “username” and “password” in the body text.)*

---

#### **9. `cache:`**

View the cached version of a webpage.

- **Syntax:**

  ```
  cache:[URL]
  ```

- **Example:**

  ```
  cache:example.com
  ```

  *(View Google’s cached version of example.com.)*

---

#### **10. `ext:` (Same as `filetype:`)**

Search for files with specific extensions.

- **Syntax:**

  ```
  ext:[extension]
  ```

- **Example:**

  ```
  ext:doc confidential
  ```

  *(Find `.doc` files containing the word “confidential.”)*

---

#### **11. `related:`**

Find websites related to a given site.

- **Syntax:**

  ```
  related:[URL]
  ```

- **Example:**

  ```
  related:example.com
  ```

  *(Find sites similar to example.com.)*

---

#### **12. `link:`**

Find pages that link to a specific site.

- **Syntax:**

  ```
  link:[URL]
  ```

- **Example:**

  ```
  link:example.com
  ```

  *(Find pages linking to example.com.)*

---

#### **13. `-` (Exclude Results)**

Exclude specific words, phrases, or operators.

- **Syntax:**

  ```
  -[operator or keyword]
  ```

- **Example:**

  ```
  site:example.com -filetype:pdf
  ```

  *(Search within example.com but exclude PDF files.)*

---

#### **14. `" "` (Exact Match)**

Search for an exact phrase or word.

- **Syntax:**

  ```
  "exact phrase"
  ```

- **Example:**

  ```
  "login portal"
  ```

  *(Find pages containing the exact phrase “login portal.”)*

---

#### **15. `OR`**

Combine two search conditions.

- **Syntax:**

  ```
  [condition1] OR [condition2]
  ```

- **Example:**

  ```
  intitle:"admin" OR inurl:"admin"
  ```

  *(Find pages with "admin" in the title or URL.)*

---

#### **16. `*` (Wildcard)**

Use a wildcard to fill in unknown words or characters.

- **Syntax:**

  ```
  "keyword * keyword"
  ```

- **Example:**

  ```
  "site login * portal"
  ```

  *(Find variations like "site login admin portal" or "site login user portal.")*

---

#### **17. Combining Multiple Operators**

You can combine operators for more specific searches.

- **Example:**

  ```
  site:example.com filetype:pdf intitle:"confidential"
  ```

  *(Find PDF files on example.com with “confidential” in the title.)*

---

### **General Tips**

- Use **logical grouping** with parentheses to refine results.
  - **Example:**

    ```
    (intitle:"index of" | inurl:admin) filetype:txt
    ```

- Experiment with combinations to achieve more targeted results.

---
