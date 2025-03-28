# CryptoImage – README

**CryptoImage** is a simple client-side web application that embeds an encrypted file into a PNG image (optionally using a “cover image” for steganography). It also protects the file using a combination of RSA and AES encryption, albeit with some important caveats described below.

---

## Table of Contents
1. [Getting Started](#getting-started)
2. [Usage](#usage)
   1. [Encrypting a File](#encrypting-a-file)
   2. [Decrypting a File](#decrypting-a-file)
3. [Security Warnings](#security-warnings)
4. [License (GPLv3)](#license-gplv3)

---

## Getting Started

1. **Download the `index.html`** and save it on your computer.
2. **Open `index.html`** in your web browser.  
   The application runs purely on the client side – no external servers needed.

Make sure your browser supports the **Web Crypto API** (for RSA and AES operations). Modern browsers such as Chrome, Firefox, Safari, and Edge generally do.

---

## Usage

### Encrypting a File

1. **Select the file** you want to encrypt by clicking the “Choose File” button (labeled *File to encrypt* in the UI).
2. *(Optional)* **Select a cover image** by clicking the “cover image” file input (labeled *Optional custom image*).  
   - If you do not provide a cover image, the application will create a blank canvas image to embed the encrypted data.
   - If you do provide a cover image, make sure it’s large enough (in pixel count) to hold the encrypted content.
3. Click the **“Encrypt”** button.
4. You will be prompted for a **password** to encrypt the RSA private key. Enter it carefully.  
   > This password is used to derive an AES key via PBKDF2, which then encrypts the RSA private key. The RSA key pair is used to encrypt the file’s AES key.
5. After a few seconds, a **download prompt** will appear, letting you save the resulting PNG image.  
   - The newly generated PNG contains all the encrypted data (both the file and the necessary metadata to decrypt it later).

### Decrypting a File

1. **Select the PNG** image (the one generated from the encryption step) by clicking the “Choose File” button.
2. Click the **“Decrypt”** button.
3. You will be prompted for the **password** you used during encryption.  
4. After a moment, you should see a **download prompt** allowing you to save the **decrypted file** under its original name.

---

## Security Warnings

1. **Everything is stored in the PNG**:  
   - The RSA private key (encrypted with your password) and the AES key (encrypted with the RSA public key) are both embedded in the PNG.  
   - An attacker can extract these items offline and attempt to brute-force the password for the private key.
2. **Password strength is critical**:  
   - Since the application uses PBKDF2, it can slow down brute-force attacks, but if your password is weak, an attacker can recover it by trying many combinations offline (with no rate limits).
3. **No server-side checks**:  
   - There are no rate-limiting, account lockouts, or multi-factor authentication mechanisms. Everything happens locally in the browser.
4. **Use a large cover image**:  
   - If the cover image is too small, the application may fail to embed the encrypted data.

---

## License (GPLv3)

This project is licensed under the terms of the **GNU General Public License v3.0**.  
You can find the full text of the license [here](https://www.gnu.org/licenses/gpl-3.0.html).

