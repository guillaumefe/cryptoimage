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
5. [Entropy Game: Enhancing Encryption Security](#entropy-game-enhancing-encryption-security)
   1. [What is the Entropy Game?](#51-what-is-the-entropy-game)
   2. [How It Works](#52-how-it-works)
   3. [Security Benefits](#53-security-benefits)
6. [License (GPLv3)](#license-gplv3)

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

> **Tip**: You may want to rename the resulting PNG.

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

## 5. Entropy Game: Enhancing Encryption Security
CryptoImage incorporates an interactive feature called the Entropy Game, designed to enhance the security of cryptographic key generation by increasing entropy during the encryption process.

### 5.1 What is the Entropy Game?
The Entropy Game is a simple, interactive mini-game that activates automatically when encryption begins. Users are presented with falling water droplets on the screen and are instructed to click them. Every click—successful or not—generates additional randomness (entropy), which is directly fed into the cryptographic functions.

### 5.2 How It Works
Activation:
As soon as you enter your encryption password and confirm, the Entropy Game starts immediately.

**Gameplay:**

- Water droplets fall randomly across the browser window.
- Users are encouraged to click on these droplets. Each successful click increments an entropy score visibly displayed on the page.
- Even clicks slightly outside the droplets count as successful to facilitate ease of use and avoid frustration.
- All clicks (whether they hit a droplet or not) are captured and used to enrich the entropy pool, enhancing randomness.

**Pause/Resume:**
A pause button appears during encryption, allowing users to temporarily halt the game without interrupting the encryption process itself. The encryption will continue, but entropy collection pauses until the game resumes.

**Completion:**
The game and all related UI elements automatically disappear when encryption is completed.

### 5.3 Security Benefits
The primary security advantage of the Entropy Game lies in improved randomness during the generation of cryptographic keys (RSA and AES). User-generated entropy provides an additional, dynamic source of randomness beyond the browser's built-in random number generator (crypto.getRandomValues()), significantly reducing predictability and enhancing resistance to potential cryptographic attacks.

This interactive entropy collection process makes it substantially more difficult for attackers to predict or brute-force cryptographic keys, offering greater overall protection for encrypted data.

---

## 6. License (GPLv3)

CryptoImage is licensed under the **GNU General Public License v3.0**.  
You can find the full text of the license [here](https://www.gnu.org/licenses/gpl-3.0.html).

Feel free to modify and distribute this project under the terms of the GPLv3. If you make improvements, consider contributing them back to the community!

---

**Happy encrypting!** If you have feedback or encounter any issues, please open an issue or share your thoughts.
