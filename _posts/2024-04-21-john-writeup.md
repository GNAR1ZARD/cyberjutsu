---
layout: post
title: "Hash Cracking with John"
date: 2024-04-21
---

## Table of Contents

1. [Introduction to Hash Cracking with John the Ripper](#introduction-to-hash-cracking-with-john-the-ripper)
2. [What are Hashes?](#what-are-hashes)
3. [What Makes Hashes Secure?](#what-makes-hashes-secure)
4. [Role of John the Ripper](#role-of-john-the-ripper)
5. [Cracking Basic Hashes](#cracking-basic-hashes)
    1. [John Basic Syntax](#john-basic-syntax)
    2. [Automatic Cracking](#automatic-cracking)
    3. [Identifying Hashes](#identifying-hashes)
    4. [Format-Specific Cracking](#format-specific-cracking)
    5. [A Note on Formats](#a-note-on-formats)
        1. [Standard Hash Types](#standard-hash-types)
        2. [Non-Standard Hash Types](#non-standard-hash-types)
6. [Cracking Windows Hashes with John the Ripper](#cracking-windows-hashes-with-john-the-ripper)
    1. [Understanding NTHash / NTLM](#understanding-nthash--ntlm)
    2. [Dumping the Hashes](#dumping-the-hashes)
    3. [Hash Cracking Strategy](#hash-cracking-strategy)
        1. [Wordlist Attack](#wordlist-attack)
        2. [Brute Force Attack](#brute-force-attack)
    4. [Advanced Techniques](#advanced-techniques)
        1. [Combination Attacks](#combination-attacks)
        2. [Rule-Based Attacks](#rule-based-attacks)
    5. [Utilizing John's Capabilities](#utilizing-johns-capabilities)
7. [Cracking Hashes from /etc/shadow with John the Ripper](#cracking-hashes-from-etcshadow-with-john-the-ripper)
    1. [Accessing the /etc/shadow File](#accessing-the-etcshadow-file)
        1. [Obtaining Hashes](#obtaining-hashes)
    2. [Unshadowing for John the Ripper](#unshadowing-for-john-the-ripper)
        1. [Using Unshadow](#using-unshadow)
    3. [Cracking the Combined Hash File](#cracking-the-combined-hash-file)
        1. [Initiating Password Cracking](#initiating-password-cracking)
        2. [Monitoring Progress](#monitoring-progress)
8. [Single Crack Mode in John the Ripper](#single-crack-mode-in-john-the-ripper)
    1. [Understanding Single Crack Mode](#understanding-single-crack-mode)
    2. [Leveraging GECOS Fields](#leveraging-gecos-fields)
    3. [Using Single Crack Mode](#using-single-crack-mode)
        1. [Setting Up Files for Single Crack Mode](#setting-up-files-for-single-crack-mode)
        2. [Example Command](#example-command)
    4. [Key Considerations](#key-considerations)
9. [Custom Rules in John the Ripper](#custom-rules-in-john-the-ripper)
    1. [Understanding Custom Rules](#understanding-custom-rules)
    2. [Common Custom Rules](#common-custom-rules)
    3. [How to Create Custom Rules](#how-to-create-custom-rules)
        1. [Defining a Rule](#defining-a-rule)
        2. [Syntax and Modifiers](#syntax-and-modifiers)
        3. [Example Rule Configuration](#example-rule-configuration)
    4. [Using Custom Rules in Cracking](#using-custom-rules-in-cracking)
        1. [Command Example](#command-example)
    5. [Tips for Rule Creation](#tips-for-rule-creation)
10. [Cracking a Password Protected Zip File with John the Ripper](#cracking-a-password-protected-zip-file-with-john-the-ripper)
    1. [Zip2John Tool](#zip2john-tool)
        1. [Basic Usage](#basic-usage)
    2. [Cracking the Zip File](#cracking-the-zip-file)
        1. [Cracking Command](#cracking-command)
    3. [Workflow Summary](#workflow-summary)
11. [Cracking a Password Protected RAR Archive with John the Ripper](#cracking-a-password-protected-rar-archive-with-john-the-ripper)
    1. [Rar2John Tool](#rar2john-tool)
        1. [Basic Usage](#basic-usage-1)
    2. [Cracking the RAR File](#cracking-the-rar-file)
        1. [Cracking Command](#cracking-command-1)
    3. [Workflow Summary](#workflow-summary-1)
12. [Cracking SSH Key Passwords with John the Ripper](#cracking-ssh-key-passwords-with-john-the-ripper)
    1. [SSH2John Tool](#ssh2john-tool)
        1. [Basic Usage](#basic-usage-2)
    2. [Cracking the SSH Key Password](#cracking-the-ssh-key-password)
        1. [Cracking Command](#cracking-command-2)
    3. [Workflow Summary](#workflow-summary-2)
13. [Acknowledgments](#acknowledgments)
14. [Disclaimer](#disclaimer)

---

## Introduction to Hash Cracking with John the Ripper

John the Ripper is renowned for its fast cracking speed and support for a wide array of hash types, making it a favorite among security professionals. This guide assumes no prior knowledge and starts with the basics of hashing before advancing to practical cracking techniques.

### What are Hashes?

A hash transforms any length of data into a fixed-length representation, concealing the original data's value. This conversion is performed using a hashing algorithm. Popular algorithms include MD4, MD5, SHA1, and NTLM. Let's illustrate this with an example:

- **Data**: "apple"
- **Algorithm**: MD5
- **Output**: `1f3870be274f6c49b3e31a0c6728957f` (a standard 32-character MD5 hash)

- **Data**: "applesauce"
- **Algorithm**: MD5
- **Output**: `cf4c4bc60bce2cbf683015b30cccfd7b` (another 32-character MD5 hash)

These examples demonstrate how inputs of different lengths are transformed into outputs of uniform size by the same hashing algorithm.

### What Makes Hashes Secure?

Hash functions are designed as one-way functions—it's easy to generate a hash from an input, but difficult to derive the original input from its hash. This difficulty is rooted in the distinction between P and NP problems in computational complexity:

- **P (Polynomial Time)**: Includes problems that can be solved in polynomial time, such as sorting a list.
- **NP (Non-deterministic Polynomial Time)**: Includes problems where solutions can be verified quickly, but finding solutions is challenging, and no efficient algorithm is known.

This implies that while hashing (a "P" process) is computationally straightforward, reversing a hash (an "NP" process) is not feasible with standard computational resources.

### Role of John the Ripper

Despite the non-reversibility of hashing algorithms, cracking hashes is not impossible. With access to a hashed password and knowledge of the hashing algorithm used, one can perform what's known as a dictionary attack. This involves hashing a large number of potential passwords and comparing these hashes against the target hash to find a match.

John the Ripper facilitates this process by enabling fast, efficient brute force attacks across various hash types, making it a valuable tool in ethical hacking and security testing. This process, while computationally intensive, leverages John's optimization to test numerous possibilities swiftly, identifying potential password matches effectively.

## Cracking Basic Hashes

John the Ripper is a versatile tool for cracking hashes, suitable for both beginners and advanced users. This section explores the basics of John's functionality and provides guidelines on how to effectively utilize this tool for cracking various types of hashes.

#### John Basic Syntax

- **john [options] [path to file]**:
  - **john**: Command to invoke John the Ripper.
  - **[options]**: Modifiers that customize the cracking process, such as selecting a wordlist or specifying a hash format.
  - **[path to file]**: Path to the file containing the hashes to be cracked. If the file is in the current directory, only the filename is needed.

#### Automatic Cracking

John can automatically detect hash types and choose the best cracking strategies, although this may sometimes be unreliable. For a straightforward attempt:

- **--wordlist=[path to wordlist]**:
  - Enables wordlist mode, using the specified wordlist file for password attempts.
- **Example Command**:
  - `john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`

#### Identifying Hashes

If automatic detection fails, you can identify hash types manually using tools like `hash-identifier`. This Python tool is straightforward:

- Download with:
  - `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`
- Run with:
  - `python3 hash-id.py`
- Then input the hash to receive possible hash formats.

#### Format-Specific Cracking

Once the hash type is determined, specify it in John's command to improve accuracy:

- **--format=[format]**:
  - Informs John of the hash format to use.
- **Example Command**:

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

### A Note on Formats

Hash functions are categorized into standard and non-standard types, used for securing passwords and data integrity. To determine the correct format to use with John the Ripper, especially when specifying standard hash types, you may need to list all available formats. You can do this using the command:

```bash
john --list=formats
```

To find out if a specific hash type like MD5 requires a prefix (e.g., "raw-"), you can filter the list of formats by running:

```bash
john --list=formats | grep -iF "md5"
```

This command helps identify whether the "raw-" prefix is necessary for standard hash types in John's command syntax.

#### Standard Hash Types

Examples include:

- **MD5**: Once common, now vulnerable.
- **SHA-1**: Being phased out due to vulnerabilities.
- **SHA-256** and **SHA-3**: Part of the SHA-2 and SHA-3 families, these are secure and widely used.

These hashes are standardized by organizations like NIST and are supported by many systems.

#### Non-Standard Hash Types

These might be proprietary or less commonly used hashes, not openly documented or standardized. For non-standard hashes, you may need custom solutions or plugins.

## Cracking Windows Hashes with John the Ripper

John the Ripper, a powerful tool for cracking passwords, can be used for more complex scenarios, such as cracking Windows hashes during penetration tests or red team engagements. Here, we'll focus on NTHash (NTLM), commonly used to store authentication data in modern Windows operating systems.

### Understanding NTHash / NTLM

The NTHash, also known as NTLM, is the primary format for storing user and service password hashes on Windows machines. This format evolved from the older LAN Manager (LM) hashing method and is part of Microsoft's "New Technology" line which began with Windows NT.

To acquire NTHash/NTLM hashes, you can use tools like Mimikatz to dump the Security Account Manager (SAM) database or extract them from the NTDS.dit file in Active Directory. While "pass the hash" attacks can bypass the need for password cracking, understanding how to crack these hashes is crucial when facing weak password policies.

#### Dumping the Hashes

To start cracking, you first need to obtain the hashes:

- **Using Mimikatz**:
  - Extract hashes from the SAM with:

    ```bash
    mimikatz # privilege::debug
    mimikatz # sekurlsa::logonpasswords
    ```

- **From NTDS.dit**:
  - Extract hashes using tools like NTDSUtil or DSInternals.

#### Hash Cracking Strategy

Once you have the hashes, you can begin cracking:

- **Wordlist Attack**:
  - Use a comprehensive wordlist to attempt common passwords.
  - **Example Command**:

    ```bash
    john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm_hashes.txt
    ```

- **Brute Force Attack**:
  - Use brute force when wordlist attacks fail, specifying character sets and length.
  - **Example Command**:

    ```bash
    john --format=nt --incremental=Alnum ntlm_hashes.txt
    ```

### Advanced Techniques

For a more targeted approach, you can also consider:

- **Combination Attacks**:
  - Combine words from different wordlists or with common substitutions and suffixes.
  - **Example Command**:

    ```bash
    john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt --rules=Best64 ntlm_hashes.txt
    ```

- **Rule-Based Attacks**:
  - Apply transformation rules to modify base words from a wordlist.
  - **Example Command**:

    ```bash
    john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt --rules ntlm_hashes.txt
    ```

### Utilizing John's Capabilities

To maximize the effectiveness of John the Ripper in cracking NTLM hashes, it's essential to tailor the tool's options and strategies to the specifics of the hash and the context of the environment. Here are some guidelines:

- **Customizing Options**:
  - Adjust performance settings and use the optimal format flags (`--format=nt`).
  
- **Hash Type Specification**:
  - Always specify the hash type to improve cracking efficiency and accuracy.

- **Monitoring and Managing Sessions**:
  - Use John's session management features to pause and resume long cracking sessions efficiently.

Cracking NTLM hashes with John the Ripper can be a potent method in a security professional's toolkit, particularly when dealing with compromised systems or during a security audit. It's a crucial skill for ethical hackers, emphasizing the importance of robust password policies and system security measures.

## Cracking Hashes from /etc/shadow with John the Ripper

In Linux systems, the `/etc/shadow` file is crucial as it stores password hashes alongside metadata like password change dates and expiration details. Access to this file is restricted to the root user, highlighting the need for elevated privileges to leverage its contents for password cracking.

### Accessing the /etc/shadow File

To begin cracking passwords stored in `/etc/shadow`, you first need to obtain this file, which requires root access. Here are the steps to access and prepare these hashes for cracking:

#### Obtaining Hashes

- **Secure Shell (SSH) or Physical Access**:
  - Log in as root or use sudo to access the file:

    ```bash
    sudo cat /etc/shadow
    ```

- **Copying Files for Analysis**:
  - Transfer `/etc/shadow` and `/etc/passwd` to your local machine for safe analysis:

    ```bash
    scp root@target:/etc/shadow local_directory/
    scp root@target:/etc/passwd local_directory/
    ```

### Unshadowing for John the Ripper

John the Ripper requires a specific format to process the hashes from Linux systems. The `unshadow` tool within the John suite is used to merge `/etc/passwd` and `/etc/shadow` into a single file that John can interpret.

#### Using Unshadow

- **Syntax**:
  - **unshadow [path to passwd] [path to shadow]**:
    - **unshadow**: Command to invoke the unshadow tool.
    - **[path to passwd]**: Path to the `/etc/passwd` file.
    - **[path to shadow]**: Path to the `/etc/shadow` file.

- **Example Command**:
  - Combine the files into a format suitable for cracking:

    ```bash
    unshadow /path/to/passwd /path/to/shadow > combined.txt
    ```

### Cracking the Combined Hash File

With the combined file ready, you can use John the Ripper's powerful cracking techniques to attempt to decrypt the hashes.

#### Initiating Password Cracking

- **Wordlist Attack**:
  - Use a common wordlist to automate password guessing.
  - **Example Command**:

    ```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt combined.txt
    ```

- **Rule-Based Attack**:
  - Apply modifications to wordlist entries to match common password variations.
  - **Example Command**:

    ```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt --rules combined.txt
    ```

#### Monitoring Progress

- **Show Cracked Passwords**:
  - To view the passwords that have been successfully cracked:

    ```bash
    john --show combined.txt
    ```

- **Manage Sessions**:
  - John the Ripper allows you to save progress and resume later, which is useful for lengthy cracking sessions:

    ```bash
    john --session=mycrack --wordlist=/usr/share/wordlists/rockyou.txt combined.txt
    john --restore=mycrack
    ```

Cracking passwords from the `/etc/shadow` file is a complex but achievable task when proper tools and techniques are employed. This process emphasizes the importance of secure password policies and system security, particularly on systems where root access can provide extensive control and access to sensitive data.

## Single Crack Mode in John the Ripper

John the Ripper's Single Crack mode is an efficient method for cracking passwords using heuristic techniques based on the usernames. This mode is particularly useful when the password is likely to be a simple variation of the username.

### Understanding Single Crack Mode

Single Crack mode utilizes the username and any associated metadata to generate possible passwords. It relies on word mangling—a technique where John modifies the base word (username) according to predefined "mangling rules."

#### Word Mangling Example

Consider the username "Markus." Here are some password variations John might try:

- Simple numerical increments: `Markus1`, `Markus2`, `Markus3`, etc.
- Case variations: `MArkus`, `MARkus`, `MARKus`, etc.
- Special character appendages: `Markus!`, `Markus$`, `Markus*`, etc.

This method builds a dynamic dictionary tailored to the specific details of the target, exploiting common weak-password practices.

### Leveraging GECOS Fields

In UNIX and Linux systems, user account details are stored in the `/etc/passwd` file, which includes fields separated by colons (" : "). These fields are known as GECOS fields, containing user metadata like full name and home directory, which can be invaluable for Single Crack mode.

#### Example of Utilizing GECOS

When cracking `/etc/shadow` hashes, incorporating GECOS data can enhance the wordlist by including elements relevant to the user's personal information. This approach leverages potential password clues embedded in the user's full name or other GECOS details.

### Using Single Crack Mode

To initiate Single Crack mode, the syntax is straightforward but requires some preparation to ensure John interprets the data correctly.

#### Setting Up Files for Single Crack Mode

For John to process hashes in Single Crack mode effectively, the input file must pair each hash with its corresponding username. This contextual linkage guides John's heuristic password generation process.

- **File Preparation**:
  - Original hash file entry: `1efee03cdcb96d90ad48ccc7b8666033`
  - Modified for Single Crack mode: `mike:1efee03cdcb96d90ad48ccc7b8666033`

#### Example Command

- **Syntax**:
  - `john --single --format=[format] [path to file]`
  - **`--single`**: This flag specifies Single Crack mode.
  - **`--format=[format]`**: Indicates the hash format (e.g., `raw-sha256`).
  - **`[path to file]`**: Path to the prepared hash file.

- **Using the Command**:

  ```bash
  john --single --format=raw-sha256 hashes.txt
  ```

### Key Considerations

Single Crack mode is a powerful option when dealing with potentially weak passwords directly derived from user-specific data. It emphasizes the importance of strong password policies and the dangers of using predictable password conventions based on user information.

By exploiting the common tendency of users to create passwords based on personal details, Single Crack mode offers a sophisticated method to breach weak security practices, making it a crucial technique for security professionals in their toolkit.

## Custom Rules in John the Ripper

John the Ripper allows for the creation of custom rules to dynamically generate passwords using known password structures or common patterns. This is particularly useful when you have insights into the password habits of your target.

### Understanding Custom Rules

Custom rules in John are a powerful feature that leverages your understanding of typical password compositions to enhance cracking efforts. These rules are defined in John’s configuration file and can manipulate a base word from your wordlist according to specified patterns.

### Common Custom Rules

Many organizations enforce password complexity requirements to prevent simple dictionary attacks. For example, a password might need to include a capital letter, a number, and a symbol. Users often comply in predictable ways, such as:

**Example Password**: `Tigerpassword1!`

Here, the pattern consists of starting with a capital letter, ending with a number followed by a symbol. This predictable structure can be exploited to create effective custom rules.

### How to Create Custom Rules

Custom rules are typically defined in the `john.conf` file, found under `/etc/john/john.conf` or `/opt/john/john.conf` depending on your installation.

#### Defining a Rule

- **Rule Name**:
  - `[List.Rules:ExampleRules]` - This header defines a new set of rules named "ExampleRules."

#### Syntax and Modifiers

- **Modifiers**:
  - `Az` - Append characters to the end of the word.
  - `A0` - Prepend characters to the beginning of the word.
  - `c` - Capitalize a specific character in the word.

- **Character Sets**:
  - `[0-9]` - Includes all numbers from 0 to 9.
  - `[A-Z]` - Only uppercase letters.
  - `[a-z]` - Only lowercase letters.
  - `[!£$%@]` - Specific symbols.

#### Example Rule Configuration

To create a password like `Tigerpassword1!` from the base word `tigerpassword`, your custom rule in `john.conf` might look like this:

```
[List.Rules:TigerPassword]
cAz"[0-9] [!£$%@]"
```

This rule configuration does the following:

- **c** - Capitalizes the first letter of `tigerpassword` to make it `Tigerpassword`.
- **Az** - Appends characters defined in the subsequent character set.
- **"[0-9] [!£$%@]"** - Appends a number followed by a symbol from the set.

### Using Custom Rules in Cracking

Once you've defined your rule, you can invoke it using the `--rules` option in John.

#### Command Example

```bash
john --wordlist=[path to wordlist] --rules=TigerPassword [path to file]
```

### Tips for Rule Creation

When writing custom rules, verbalizing the pattern can help clarify the structure, much like composing regular expressions. Additionally, reviewing pre-existing rules in the John the Ripper rule set can provide further insights and examples.

John's flexibility with custom rules makes it an invaluable tool for cracking passwords, especially when you understand the common password patterns of your target audience. This strategic approach allows for more targeted and efficient cracking attempts, significantly enhancing the probability of uncovering valid passwords.

## Cracking a Password Protected Zip File with John the Ripper

John the Ripper is not only effective for cracking hashes but can also tackle password-protected Zip files. This process involves converting the Zip file into a hash format that John can understand using a tool called `zip2john`.

### Zip2John Tool

`zip2john` is part of the John the Ripper suite and is designed to extract hash data from Zip files. This tool translates the encryption on the Zip file into a format readable by John the Ripper, enabling it to attempt password cracking.

#### Basic Usage

The command structure for `zip2john` is straightforward, mirroring the simplicity of other commands in the suite:

- **Command Format**:

  ```bash
  zip2john [options] [zip file] > [output file]
  ```
  
- **Parameters**:
  - **[options]**: Optional parameters for handling specific checksums or other advanced settings.
  - **[zip file]**: The path to the Zip file you are targeting.
  - **>**: Directs the output to the specified file rather than displaying it.
  - **[output file]**: The file where the extracted hash will be stored.

- **Example Command**:

  ```bash
  zip2john zipfile.zip > zip_hash.txt
  ```

### Cracking the Zip File

After generating the hash file with `zip2john`, you can use John the Ripper to attempt to crack the password. This process uses familiar syntax and techniques already discussed for other types of files.

#### Cracking Command

Using the generated hash file, the command to start cracking the password of the Zip file is:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

This command utilizes a wordlist (`rockyou.txt`) to perform a dictionary attack against the hash representing the Zip file's password.

### Workflow Summary

1. **Extract the Hash**:
   - Use `zip2john` to convert the Zip file into a hash format. Ensure the output is redirected to a file.

2. **Crack the Password**:
   - Apply John the Ripper with a suitable wordlist to the hash file, attempting to reveal the password.

### Practical Considerations

When cracking password-protected Zip files, consider the following:

- **Wordlist Quality**: The success of cracking depends heavily on the quality and relevance of the wordlist used.
- **Resource Allocation**: Password cracking can be resource-intensive. Adjusting the performance settings of John the Ripper can optimize the process without overwhelming your system.

Cracking Zip file passwords with John the Ripper expands your toolkit's versatility, providing significant capabilities in digital forensics and security testing scenarios.

## Cracking a Password Protected RAR Archive with John the Ripper

Similar to Zip files, RAR archives—created by the WinRAR archive manager—can also be cracked using John the Ripper. This process involves converting the RAR file into a hash format using the `rar2john` tool, enabling John to attempt to crack the password.

### Rar2John Tool

`rar2john` is akin to `zip2john` and is tailored for extracting hash data from RAR files. This tool converts the RAR file's encryption into a hash format readable by John the Ripper.

#### Basic Usage

The command structure for `rar2john` is straightforward and designed for ease of use:

- **Command Format**:

  ```bash
  rar2john [rar file] > [output file]
  ```

- **Parameters**:
  - **rar2john**: Command to invoke the rar2john tool.
  - **[rar file]**: The path to the RAR file you are targeting.
  - **>**: Directs the output to the specified file.
  - **[output file]**: The file where the extracted hash will be stored.

- **Example Command**:

  ```bash
  rar2john rarfile.rar > rar_hash.txt
  ```

### Cracking the RAR File

With the hash file prepared using `rar2john`, you can proceed to use John the Ripper to crack the RAR archive's password. This utilizes a similar approach as described for Zip files.

#### Cracking Command

The following command uses a wordlist to perform a dictionary attack against the hash extracted from the RAR file:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

This command takes advantage of the `rockyou.txt` wordlist, aiming to match one of its entries with the password encrypted in the RAR archive.

### Workflow Summary

1. **Extract the Hash**:
   - Use `rar2john` to convert the RAR file into a suitable hash format. Ensure you redirect the output to a file for use in cracking.

2. **Crack the Password**:s
   - Apply John the Ripper with an appropriate wordlist to the hash file, attempting to discover the encrypted password.

Cracking RAR file passwords extends the capabilities of John the Ripper into the realm of archive files, reinforcing its utility in security testing and digital forensics.

## Cracking SSH Key Passwords with John the Ripper

Expanding the repertoire further, John the Ripper can also crack the password of SSH private key files, specifically the id_rsa type used for SSH key-based authentication. This method is useful in scenarios where you might need to recover or bypass the passphrase protection of an SSH key, commonly encountered in Capture The Flag (CTF) challenges and security audits.

### SSH2John Tool

`ssh2john` is a conversion tool that processes SSH private keys into a hash format that John the Ripper can handle. This functionality underscores John's adaptability across different encryption and authentication methods.

#### Basic Usage

The usage of `ssh2john` follows a consistent pattern seen in other conversion tools in the John the Ripper suite:

- **Command Format**:

  ```bash
  ssh2john [id_rsa private key file] > [output file]
  ```

- **Parameters**:
  - **ssh2john**: Command to invoke the ssh2john tool.
  - **[id_rsa private key file]**: The path to the id_rsa file to be processed.
  - **>**: Directs the output to the specified file.
  - **[output file]**: The file that will store the extracted hash.

- **Alternative Command Using Python**:
  - If `ssh2john` is not installed as a standalone tool, you can use the Python script version:

    ```bash
    python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```

    Or on Kali Linux:

    ```bash
    python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```

- **Example Command**:

  ```bash
  ssh2john id_rsa > id_rsa_hash.txt
  ```

### Cracking the SSH Key Password

Once the hash is extracted and stored in a file, you can use John the Ripper to attempt to crack the passphrase of the SSH key.

#### Cracking Command

This command employs a wordlist attack to decrypt the passphrase protected SSH key:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

### Workflow Summary

1. **Extract the Hash**:
   - Use `ssh2john` or the corresponding Python script to convert the SSH private key into a hash format suitable for John the Ripper.
   - Ensure that the output is correctly redirected to a file for further processing.

2. **Crack the Password**:
   - Use John the Ripper with an appropriate wordlist to attempt to uncover the passphrase.

Cracking SSH key passwords is a testament to John the Ripper's versatility, providing crucial capabilities for security testing and ethical hacking. This technique is essential for scenarios where key-based SSH access needs to be recovered or secured against potential passphrase vulnerabilities.

### Acknowledgments

The structure of this guide was inspired by the TryHackMe module created by PoloMints, available at [https://tryhackme.com/r/room/johntheripper0](https://tryhackme.com/r/room/johntheripper0).

### Disclaimer

**Legal Considerations:** Always ensure you have permission to perform hash/password cracking on the network you are targeting to avoid violating legal or ethical boundaries.

## Glossary

Below is a comprehensive list of commands and their descriptions, organized alphabetically.

| Command                          | Description                                                              |
|----------------------------------|--------------------------------------------------------------------------|
| `john`                           | Command to invoke John the Ripper, used for password cracking.            |
| `john --wordlist=[path]`         | Enables wordlist mode using the specified wordlist file for password attempts. |
| `john --format=[format]`         | Informs John of the hash format to use.                                   |
| `john --list=formats`            | Lists all available hash formats that John the Ripper can crack.          |
| `john --single --format=[format] [path to file]` | Uses Single Crack mode, specifying the hash format and the path to the prepared hash file. |
| `john --show [path to file]`     | Displays the passwords that have been successfully cracked.              |
| `john --session=[session name] --wordlist=[path to wordlist] [path to file]` | Saves progress in a session for later resumption. |
| `john --restore=[session name]`  | Restores a saved cracking session.                                        |
| `john --rules=[rule name] --wordlist=[path to wordlist] [path to file]` | Applies custom rules during password cracking.                        |
| `unshadow [path to passwd] [path to shadow]` | Combines `/etc/passwd` and `/etc/shadow` files into a single format for John to interpret. |
| `zip2john [zip file] > [output file]` | Converts a Zip file into a hash format readable by John the Ripper.      |
| `rar2john [rar file] > [output file]` | Converts a RAR file into a hash format readable by John the Ripper.      |
| `ssh2john [id_rsa private key file] > [output file]` | Converts an SSH private key into a hash format readable by John the Ripper. |