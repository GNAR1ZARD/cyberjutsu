---

layout: post  
title: "Scripting for Security"  
date: 2024-12-03
categories: Sys-Admin

---

## **Table of Contents**

1. [Introduction to Bash and Python Scripting](#introduction-to-bash-and-python-scripting)
2. [Bash Scripting Basics](#bash-scripting-basics)
   1. [Basic Syntax and Variables](#basic-syntax-and-variables)
   2. [File Management](#file-management)
   3. [Environment Variables and Permissions](#environment-variables-and-permissions)
   4. [Command-Line Arguments and Loops](#command-line-arguments-and-loops)
   5. [Networking and System Monitoring](#networking-and-system-monitoring)
3. [Python Scripting Basics](#python-scripting-basics)
   1. [Essential Modules for Cybersecurity](#essential-modules-for-cybersecurity)
   2. [Working with Files and Directories](#working-with-files-and-directories)
   3. [Network Operations with `socket`](#network-operations-with-socket)
   4. [Automating with `subprocess`](#automating-with-subprocess)
   5. [Handling JSON and YAML Configuration Files](#handling-json-and-yaml-configuration-files)
4. [Intermediate Scripting Techniques](#intermediate-scripting-techniques)
   1. [Error Handling and Validation](#error-handling-and-validation)
   2. [Logging and Timestamping](#logging-and-timestamping)
   3. [Argument Parsing](#argument-parsing)
5. [Advanced Scripting for Cybersecurity](#advanced-scripting-for-cybersecurity)
   1. [Port Scanners](#port-scanners)
   2. [File Integrity Checkers](#file-integrity-checkers)
   3. [Activity and Intrusion Detection](#activity-and-intrusion-detection)
6. [Best Practices for Cybersecurity Scripts](#best-practices-for-cybersecurity-scripts)
   1. [Shebang and File Permissions](#shebang-and-file-permissions)
   2. [Virtual Environments](#virtual-environments)
   3. [Code Readability and Comments](#code-readability-and-comments)
7. [Glossary](#glossary)
8. [Further Reference Examples](#8-further-reference-examples)

---

## **1. Introduction to Bash and Python Scripting**

Scripting is a cornerstone of cybersecurity operations, enabling automation of tasks like network scanning, log analysis, and system monitoring. **Bash** provides simplicity and direct OS interaction, while **Python** adds flexibility and power for complex workflows.

---

## **2. Bash Scripting Basics**

### **2.1 Basic Syntax and Variables**

**Declaring Variables**:

```bash
#!/bin/bash
my_var="Hello, World"
echo $my_var  # Output: Hello, World
```

**Command Substitution**:

```bash
current_dir=$(pwd)
echo "Current directory: $current_dir"
```

---

### **2.2 File Management**

**Creating and Managing Files**:

```bash
touch example.txt  # Create an empty file
echo "Some text" > example.txt  # Write to file
cat example.txt  # Display file content
```

**Checking File Existence**:

```bash
if [ -f example.txt ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

---

### **2.3 Environment Variables and Permissions**

**Viewing and Modifying Environment Variables**:

```bash
echo $HOME  # Display home directory
export MY_VAR="value"  # Set an environment variable
echo $MY_VAR
```

**Changing Permissions**:

```bash
chmod 775 script.sh  # Grant execute permission
```

---

### **2.4 Command-Line Arguments and Loops**

**Accessing Arguments**:

```bash
echo "First argument: $1"
echo "All arguments: $@"
```

**Loops**:

```bash
for file in *.txt; do
    echo "File: $file"
done
```

---

### **2.5 Networking and System Monitoring**

**Ping Multiple Hosts**:

```bash
hosts=("google.com" "example.com")
for host in "${hosts[@]}"; do
    ping -c 1 $host
done
```

**Checking Active Ports**:

```bash
netstat -tuln  # Display open ports
```

---

## **3. Python Scripting Basics**

### **3.1 Essential Modules for Cybersecurity**

- **`os`**: File and directory operations.
- **`socket`**: Networking.
- **`subprocess`**: Command execution.
- **`json` & `yaml`**: Configuration files.

---

### **3.2 Working with Files and Directories**

**List Files**:

```python
import os
print(os.listdir("."))
```

**Read and Write Files**:

```python
with open("example.txt", "w") as f:
    f.write("Some text")
```

---

### **3.3 Network Operations with `socket`**

**Get Hostname and IP**:

```python
import socket
print(socket.gethostname(), socket.gethostbyname(socket.gethostname()))
```

---

### **3.4 Automating with `subprocess`**

**Run Commands**:

```python
import subprocess
result = subprocess.run(["ls", "-l"], capture_output=True, text=True)
print(result.stdout)
```

---

### **3.5 Handling JSON and YAML Configuration Files**

**JSON**:

```python
import json
config = {"user": "admin", "role": "cybersec"}
with open("config.json", "w") as f:
    json.dump(config, f, indent=4)
```

**YAML**:

```python
import yaml
config = {"user": "admin", "role": "cybersec"}
with open("config.yaml", "w") as f:
    yaml.dump(config, f)
```

---

## **4. Intermediate Scripting Techniques**

### **4.1 Error Handling and Validation**

**Python Example**:

```python
try:
    with open("nonexistent.txt", "r") as f:
        print(f.read())
except FileNotFoundError:
    print("File not found!")
```

**Bash Example**:

```bash
if ! [ -f example.txt ]; then
    echo "File not found!"
fi
```

---

### **4.2 Logging and Timestamping**

**Python Logging**:

```python
from datetime import datetime
now = datetime.now()
print(f"[{now}] Log entry")
```

**Bash Logging**:

```bash
echo "$(date): Log entry" >> log.txt
```

---

### **4.3 Argument Parsing**

**Python Argument Parser**:

```python
import argparse
parser = argparse.ArgumentParser()
parser.add_argument("filename")
args = parser.parse_args()
print(f"File: {args.filename}")
```

---

## **5. Advanced Scripting for Cybersecurity**

### **5.1 Port Scanners**

**Python**:

```python
import socket
ip = "127.0.0.1"
for port in range(1, 1025):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        if s.connect_ex((ip, port)) == 0:
            print(f"Port {port} is open")
```

---

### **5.2 File Integrity Checkers**

**Python**:

```python
import hashlib
with open("example.txt", "rb") as f:
    print(hashlib.sha256(f.read()).hexdigest())
```

---

### **5.3 Activity and Intrusion Detection**

**Log Suspicious Events**:

```python
with open("activity.log", "a") as f:
    f.write(f"[{datetime.now()}] Unauthorized access attempt\n")
```

---

## **6. Best Practices for Cybersecurity Scripts**

### **6.1 Shebang and File Permissions**

- Use `#!/usr/bin/env bash` for Bash scripts and `#!/usr/bin/env python3` for Python scripts.
- Grant execute permissions with `chmod +x`.

---

### **6.2 Virtual Environments**

**Python**:

```bash
python3 -m venv myenv
source myenv/bin/activate
```

---

### **6.3 Code Readability and Comments**

**Follow these guidelines**:

- Use meaningful variable names.
- Comment critical logic.
- Keep scripts modular and reusable.

---

## **7. Glossary**

| Symbol/Command       | Description                                    |
|----------------------|------------------------------------------------|
| `$`                  | Access variables in Bash.                     |
| `@`                  | Represents all arguments in Bash.             |
| `*`                  | Wildcard for file matching.                   |
| `socket` (Python)    | Module for networking tasks.                  |
| `subprocess` (Python)| Run shell commands in Python.                 |

---
Sure! At the end of your guide, you can include a section linking to your repository:

---

### **8. Further Reference Examples**

Explore a collection of scripting examples to reference on my GitHub repository:

**[GNAR1ZARD/Scripts](https://github.com/GNAR1ZARD/Scripts.git)**

For a detailed guide on PowerShell and its scripting capabilities, please check out my guide here:  
**[PowerShell Guide](https://gnar1zard.github.io/cyberjutsu/sys-admin/Powershell-Commands/)**

---
