# CryptoImage – README

**CryptoImage** is a purely client-side web application that encrypts and embeds a file within a PNG image. It uses a hybrid cryptosystem (RSA + AES) for encryption and optionally supports steganography by embedding the encrypted content into a user-supplied “cover image.”

> **Important**: While CryptoImage can be useful for basic protection and hiding data, it is not a substitute for a thoroughly audited, production-grade security solution. **Password strength and secure handling of the output PNG are critical** for actual data protection.

---

## Table of Contents

1. [Overview](#overview)  
2. [Getting Started](#getting-started)  
3. [Usage](#usage)  
   1. [Encrypting a File](#encrypting-a-file)  
   2. [Decrypting a File](#decrypting-a-file)  
4. [Security Warnings](#security-warnings)  
5. [License (GPLv3)](#license-gplv3)

---

## 1. Overview

**CryptoImage** runs entirely in your browser using the **Web Crypto API**—no server interaction is required. When you encrypt a file, the tool:

- Generates a fresh **RSA key pair** with a **4096-bit modulus** (`modulusLength: 4096`).  
- Derives an **AES-256** key (`length: 256`) in **GCM** mode from your chosen password (using PBKDF2) to encrypt the RSA private key.  
- Encrypts the file itself with a randomly generated **AES-256-GCM** key, which is then encrypted using the *public* part of the **RSA-4096** key.  
- Optionally embeds all necessary encrypted data (file, file name, keys, metadata) into a cover image; if no cover image is provided, a blank canvas is generated to store the data.

When you decrypt the image, the process is reversed in your browser using the same password.

---

## 2. Getting Started

1. **Download or clone** this repository and locate the file `index.html`.
2. **Open `index.html`** in your web browser (Chrome, Firefox, Safari, Edge, etc.).
3. Ensure your browser supports the **Web Crypto API** (for RSA and AES operations). Most modern browsers do.

That’s it—there are no external dependencies or servers required. All encryption and decryption happen locally in your browser.

---

## 3. Usage

### 3.1 Encrypting a File

1. **Select the file** you want to encrypt by clicking the button labeled *File to encrypt*.
2. *(Optional)* **Select a cover image** labeled *Optional custom image*.  
   - If you don’t provide a cover image, a blank canvas image will be created automatically.
   - Make sure your cover image is sufficiently large (in terms of pixel dimensions) to embed the encrypted data.
3. Click the **“Encrypt”** button.
4. Enter a **password** when prompted. This password is used to derive an **AES-256-GCM** key (via PBKDF2) that will encrypt your RSA-4096 private key.
5. After processing, your browser will prompt you to **download** a `.png` file. This PNG contains:
   - The **encrypted file**  
   - The **encrypted RSA private key**  
   - The **AES key**, which is itself encrypted with the RSA public key  
   - All other **metadata** needed for decryption

> **Tip**: You may want to rename the resulting PNG to avoid leaking the original file name in the `.png` filename.

### 3.2 Decrypting a File

1. **Select the PNG** file you created during the encryption process.
2. Click the **“Decrypt”** button.
3. When prompted, enter the **same password** used during encryption.
4. After processing, your browser will prompt you to **download the decrypted file**, automatically restoring its original name.

---

## 4. Security Warnings

1. **All essential data is stored inside the PNG**  
   - The RSA private key (encrypted with your password) and the AES key (encrypted with RSA) are embedded in the image.  
   - Anyone with the PNG can extract these items offline and brute-force the password (no rate limits).
2. **Strong Passwords Are Vital**  
   - The application uses PBKDF2 with SHA-512 and 200,000 iterations, which slows brute-force attempts but does not prevent them.  
   - A weak password (e.g., short or common) can be cracked offline. Use passphrases or sufficiently long passwords for real security.
3. **No Server-Side Protections**  
   - Everything is client-side; there’s no server-based rate limiting, account lockouts, or MFA. The security hinges entirely on your password strength and safe handling of the PNG.
4. **Steganography Is Not Foolproof**  
   - A determined adversary can still detect suspicious patterns or unexpectedly large file sizes. If your goal is high-level secrecy, additional research into robust steganographic methods is recommended.

---

## 5. License (GPLv3)

CryptoImage is licensed under the **GNU General Public License v3.0**.  
You can find the full text of the license [here](https://www.gnu.org/licenses/gpl-3.0.html).

Feel free to modify and distribute this project under the terms of the GPLv3. If you make improvements, consider contributing them back to the community!

---

**Happy encrypting!** If you have feedback or encounter any issues, please open an issue or share your thoughts.
