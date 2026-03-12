# 🔐 Secure Password Manager

A command-line password manager built in Java that stores credentials encrypted using **AES-256-GCM** encryption with **PBKDF2** key derivation.

---

## Features

- AES-256-GCM encryption for all stored credentials
- PBKDF2WithHmacSHA256 master password key derivation (100,000 iterations)
- Add, view, search, update, and delete credentials
- Strong password generator (with/without symbols)
- Password strength checker
- Export and import encrypted backup files
- Auto-backup before every save operation
- Change master password with re-encryption

---

## Project Structure

```
password-manager/
├── src/
│   └── com/
│       └── passwordmanager/
│           ├── PasswordManager.java        # Main entry point & CLI menu
│           ├── model/
│           │   └── Credential.java         # Credential data model
│           ├── handler/
│           │   └── FileHandler.java        # File I/O & backup logic
│           └── util/
│               ├── EncryptionUtil.java     # AES-256 encryption/decryption
│               └── Validator.java          # Input validation & password strength
└── data/
    ├── credentials.dat                     # Encrypted credentials (auto-created)
    └── credentials.backup                  # Auto-backup file
```

---

## Requirements

- Java 8 or higher
- No external dependencies (uses standard Java libraries only)

---

## How to Run

### Compile

```bash
javac -d out -sourcepath src src/com/passwordmanager/PasswordManager.java
```

### Run

```bash
java -cp out com.passwordmanager.PasswordManager
```

---

## First-Time Setup

On first launch, you will be prompted to create a master password.

**Master password requirements:**
- 8–128 characters
- Must include uppercase, lowercase, numbers, and symbols
- Must be rated at least "Medium" strength

> ⚠️ Your master password cannot be recovered. If lost, all stored credentials are inaccessible.

---

## Menu Options

| Option | Description |
|--------|-------------|
| 1 | Add a new credential (website, username, password, notes) |
| 2 | View all stored credentials |
| 3 | Search credentials by website or username |
| 4 | Update an existing credential's password or notes |
| 5 | Delete a credential |
| 6 | Generate a strong random password |
| 7 | Export an encrypted backup file |
| 8 | Change the master password |
| 9 | Exit (auto-saves) |
| 10 | Import/restore from a backup file |

---

## Security Details

| Property | Value |
|----------|-------|
| Encryption algorithm | AES-256-GCM |
| Key derivation | PBKDF2WithHmacSHA256 |
| PBKDF2 iterations | 100,000 |
| Salt length | 32 bytes (random per save) |
| IV length | 12 bytes (GCM standard) |
| GCM auth tag | 128 bits |

All data written to disk is encrypted. Passwords are never stored in plaintext.

---

## Backup & Recovery

- A `.backup` file is automatically created before every save operation.
- If a save fails, the previous backup is automatically restored.
- Use **Option 7** to export a portable encrypted backup.
- Use **Option 10** to import and merge credentials from a backup file.

---

## Password Generator

- Configurable length: 8–64 characters
- Always includes uppercase, lowercase, and digits
- Optional special symbols: `!@#$%^&*()_+-=[]{}|;:,.<>?`
- Uses `SecureRandom` for cryptographic randomness
- Output is Fisher-Yates shuffled for uniform distribution

---

## Input Validation Rules

| Field | Rule |
|-------|------|
| Website | Non-empty, max 200 chars |
| Username | Non-empty, max 100 chars |
| Password | Non-empty, max 128 chars |
| Master password | 8–128 chars, min strength: Medium |

---

## Limitations

- CLI only (no GUI)
- Single-user (one master password per vault)
- No cloud sync
- Credentials searched by website or username only (no tag/category support)

---

## License

This project is for educational and personal use.
