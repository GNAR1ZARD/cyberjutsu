---

layout: post  
title: "Common Command Equivalents"  
date: 2025-01-04
categories: Sys-Admin

---

Below is a quick-reference guide in Markdown that shows some of the most common Linux commands alongside their Windows Command Prompt (CMD) and PowerShell equivalents. Where a direct equivalent doesn’t exist, workarounds or closely related commands are provided.

---

## Common Command Equivalents

| **Linux** | **Windows CMD**      | **PowerShell**                       | **Description**                                                                                  |
|-----------|----------------------|--------------------------------------|--------------------------------------------------------------------------------------------------|
| **ls**    | `dir`               | `ls` or `Get-ChildItem`              | Lists directory contents.                                                                        |
| **pwd**   | `cd` (no arguments) | `Get-Location` (alias: `pwd`)        | Prints the current working directory.                                                            |
| **cd**    | `cd`                | `cd` or `Set-Location`               | Changes the current directory.                                                                   |
| **mkdir** | `mkdir`             | `mkdir` or `New-Item -ItemType Directory` | Creates a new directory.                                                                         |
| **mv**    | `move`              | `Move-Item`                          | Moves or renames files or directories.                                                           |
| **cp**    | `copy`              | `Copy-Item`                          | Copies files or directories.                                                                     |
| **rm**    | `del` (files) <br> `rmdir /s` (directories) | `Remove-Item`             | Deletes files or directories.                                                                    |
| **touch** | `type nul > filename` (creates empty file) | `New-Item -ItemType File filename` | Creates an empty file.                                                                           |
| **ln -s** | `mklink`            | `New-Item -ItemType SymbolicLink`    | Creates a symbolic link. (For directories, use `mklink /D` in CMD.)                              |
| **clear** | `cls`               | `cls` or `Clear-Host`                | Clears the terminal display.                                                                     |
| **cat**   | `type`              | `Get-Content`                        | Displays file contents.                                                                          |
| **echo**  | `echo`              | `echo` or `Write-Output`             | Prints text to the terminal.                                                                     |
| **less**  | `more`              | `more` or `Out-Host -Paging`         | Displays paged output.                                                                           |
| **man**   | *No direct equivalent* <br> `help <command>` (for built-in commands) | `Get-Help <command>`           | Accesses manual/help pages for commands.                                                         |
| **uname** | `systeminfo` <br> or `echo %OS%`       | `Get-ComputerInfo` <br> or `$PSVersionTable` | Displays basic system info.                                                                      |
| **whoami**| `whoami`            | `whoami` or `$env:USERNAME`          | Prints the current username.                                                                     |
| **tar**   | `tar` (Windows 10+ supports tar natively) <br> or 7-Zip tools | `tar` or `Compress-Archive` / `Expand-Archive` | Compresses or extracts files.                                                                    |
| **grep**  | `findstr`           | `Select-String`                      | Searches for a string within file(s) or output.                                                  |
| **head**  | *No direct equivalent* <br> (use `type file | more`)       | `Get-Content file -TotalCount <n>`                         | Returns the top N lines of a file.                                                               |
| **tail**  | *No direct equivalent* <br> (Resource Kit has `tail`)      | `Get-Content file -Tail <n>`                               | Returns the last N lines of a file.                                                              |

---

### Notes and Tips

1. **PowerShell Aliases**  
   Many PowerShell commands have aliases to make them feel more familiar to users from different environments. For example:
   - `ls` is an alias for `Get-ChildItem`.
   - `pwd` is an alias for `Get-Location`.
   - `cat` is an alias for `Get-Content`.

2. **Symbolic Links on Windows**  
   - CMD’s `mklink` can create either file or directory symlinks; use `/D` for directory symlinks.  
   - In PowerShell, `New-Item -ItemType SymbolicLink` can also create symlinks, but it often requires an elevated (Administrator) console depending on system policy.

3. **Creating Empty Files**  
   - In CMD, `type nul > filename` is a quick hack.  
   - In PowerShell, `New-Item -ItemType File filename` is more explicit.

4. **Help / Manual Pages**  
   - In CMD, `help <command>` works for built-in commands (`dir`, `del`, etc.). For external commands, you often need to consult separate documentation.  
   - In PowerShell, `Get-Help <command>` is comprehensive. Use `-Online` or `Update-Help` to fetch the latest help.

5. **System Information**  
   - For detailed OS info in CMD, `systeminfo` is the go-to.  
   - In PowerShell, `Get-ComputerInfo` provides extensive details, and `$PSVersionTable` tells you the version of PowerShell you’re running.
