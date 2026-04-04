# ZIPRACK

> **⚠️ Educational Purpose Only**  
> This tool is designed for security research, educational purposes, and authorized penetration testing only. Ensure you have explicit permission before testing any ZIP files you do not own.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.6+](https://img.shields.io/badge/Python-3.6+-blue.svg)](https://www.python.org/downloads/)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-green.svg)]

A Python-based ZIP password recovery tool that uses dictionary/wordlist attacks to brute-force password-protected ZIP archives.

---

## 📋 Table of Contents

- [Features](#-features)
- [Requirements](#-requirements)
- [Installation](#-installation)
- [Usage](#-usage)
- [Creating Encrypted ZIP Files](#-creating-encrypted-zip-files)
- [How It Works](#-how-it-works)
- [Configuration](#-configuration)
- [Examples](#-examples)
- [Troubleshooting](#-troubleshooting)
- [Legal Disclaimer](#-legal-disclaimer)
- [License](#-license)

---

## ✨ Features

- **Dictionary Attack**: Systematically tries passwords from a user-provided wordlist
- **Real-time Feedback**: Displays progress and status for each password attempt
- **Error Handling**: Gracefully handles common errors (invalid files, permissions, etc.)
- **Retry Support**: Allows multiple attempts without restarting the script
- **Cross-Platform**: Works on Linux, macOS, and Windows (with Python 3)
- **Lightweight**: No external dependencies required

---

## 📦 Requirements

| Requirement | Version |
|-------------|---------|
| Python | 3.6 or higher |
| ZIP File | Password-protected (encrypted) |
| Wordlist | Plain text file (one password per line) |

**No external packages required** – uses Python's built-in `zipfile` module.

---

## 🚀 Installation

### Option 1: Clone from Repository

```bash
git clone https://github.com/avdrevygh/ziprack
cd ziprack
```

### Option 2: Manual Download

1. Download `ziprack.py` to your desired directory
2. Ensure Python 3 is installed (`python3 --version`)
3. Prepare your wordlist file

---

## 📖 Usage

### Basic Execution

```bash
python3 ziprack.py
```

### Interactive Prompt Flow

1. **Enter the ZIP file path** when prompted:
   ```
   Enter the location of the protected ZIP archive: /path/to/protected.zip
   ```

2. **Enter the wordlist path** when prompted:
   ```
   Enter the location of the password list file: /path/to/wordlist.txt
   ```

3. The script will attempt each password and display results:
   ```
   [ATTEMPT] target 192.06.##1
   attacking password
   ---------------
   Successfull
   Cracked password: secret123
   decrypted files saved on default folder
   ```

4. **Retry option**: After completion, choose whether to try again:
   ```
   Do you want to retry? (y/n):
   ```

---

## 🔐 Creating Encrypted ZIP Files

### Using Command Line (Linux/macOS)

```bash
zip -e protected.zip filename
```

- `-e`: Enables encryption (prompts for password)
- `protected.zip`: Output ZIP file name
- `filename`: File(s) to compress

### Using GUI Tools

| OS | Method |
|----|--------|
| **Windows** | Right-click → 7-Zip → Add to archive → Set password |
| **macOS** | Use Keka or The Unarchiver with encryption |
| **Linux** | Use `zip -e` command or Archive Manager |

---

## ⚙️ How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                      ZIPRACK Workflow                        │
├─────────────────────────────────────────────────────────────┤
│  1. Load encrypted ZIP file                                  │
│  2. Load wordlist file                                       │
│  3. For each password in wordlist:                           │
│     a. Strip whitespace                                      │
│     b. Attempt extraction with password                      │
│     c. On success: Extract & report                          │
│     d. On failure: Continue to next password                 │
│  4. If all passwords fail: Report failure                    │
│  5. Optional: Retry with new files                           │
└─────────────────────────────────────────────────────────────┘
```

### Password Attempt Logic

```python
for password in wordlist:
    try:
        zf.extractall(pwd=password.encode())
        # Success - password found
    except RuntimeError:
        # Failed - try next password
```

---

## 🔧 Configuration

### Adjusting Delay Timing

By default, the script includes artificial delays for visual effect:

```python
time.sleep(0.5)  # Delay between attempts
```

**For production use**, remove or reduce these delays:

```python
# Remove these lines for faster execution:
# time.sleep(0.5)  # Line ~45
# time.sleep(0.5)  # Line ~65
```

### Wordlist Format

- One password per line
- No special formatting required
- Whitespace is automatically stripped

**Example wordlist.txt:**
```
password123
secret
qwerty
HelloWorld22
```

---

## 📚 Examples

### Example 1: Testing with Sample Files

```bash
python3 ziprack.py
# Enter: ./protected.zip
# Enter: ./wordlist.txt
```

### Example 2: Absolute Paths

```bash
python3 ziprack.py
# Enter: /home/user/archives/backup.zip
# Enter: /usr/share/wordlists/rockyou.txt
```

### Example 3: Custom Wordlist

Create your own targeted wordlist:

```bash
echo "password123" > custom_wordlist.txt
echo "admin" >> custom_wordlist.txt
echo "letmein" >> custom_wordlist.txt
python3 ziprack.py
```

---

## 🛠️ Troubleshooting

| Error | Cause | Solution |
|-------|-------|----------|
| `FileNotFoundError` | Incorrect file path | Verify path with `ls` or file explorer |
| `PermissionError` | Insufficient permissions | Use `chmod` or run with appropriate permissions |
| `Invalid ZIP file` | Corrupted or non-ZIP file | Verify file integrity with `file filename` |
| `The ZIP file is too large` | Memory constraints | Use a machine with more RAM |
| `Could not crack the password` | Password not in wordlist | Try a larger/different wordlist |
| `extractall() got an unexpected keyword argument 'pwd'` | Python version issue | Ensure Python 3.6+ is installed |

### Common Fixes

**Check Python version:**
```bash
python3 --version
```

**Verify ZIP file is encrypted:**
```bash
unzip -l protected.zip
# Look for encryption indicator
```

**Test wordlist format:**
```bash
head -n 10 wordlist.txt
```

---

## ⚖️ Legal Disclaimer

**IMPORTANT:** This tool is provided for educational and authorized security testing purposes only.

- ✅ **Allowed**: Testing ZIP files you own or have explicit written permission to test
- ❌ **Prohibited**: Unauthorized access to files, violating privacy laws, or any illegal activity

**By using this tool, you acknowledge:**
1. You are responsible for compliance with all applicable laws
2. Unauthorized access to computer systems/files is illegal
3. The authors bear no liability for misuse of this software

---

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

```
MIT License
Copyright (c) 2025 Albin Shiby
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

### Areas for Improvement
- [ ] Add progress bar for large wordlists
- [ ] Support for custom character set brute-forcing
- [ ] Multi-threading for faster attacks
- [ ] Export results to file

---

## 📞 Support

- **Issues**: Open an issue on the GitHub repository
- **Documentation**: Refer to this README for usage guidance
- **Security**: Report vulnerabilities responsibly

---

<div align="center">

**ZIPRACK** v1.01 | Created by @avdrevygh

*For educational purposes only*

</div>
