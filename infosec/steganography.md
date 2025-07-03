Lab Assignment 5: Steganography

## Author
**Name**: Emmanuel Alao  
**FOL Username**: ealao  
**Course**: INFO-6001 (Information Security)  
**Lab**: Week 9 - Steganography

---

## 📌 Objectives

This lab explores the practical application of steganography and encryption tools to securely hide and reveal data within carrier files. The focus includes:

- Hiding data inside BMP and JPG files using S-Tools and Invisible Secrets.
- Analyzing file properties and differences after embedding.
- Detecting hidden content using Xsteg.
- Applying AES encryption using `aescrypt.exe`.

---

## 🧪 Lab Breakdown

### 🔹 Part A: S-Tools

**Steps Performed**:
1. Created `Ex5` folder at root of `C:\`.
2. Copied `Lab1.bmp` to `Ex5`.
3. Created a text message (`emmanuelID.txt`) using Notepad.
4. Embedded text file into `Lab1.bmp` using S-Tools and passphrase `info6001`.
5. Selected `IDEA` encryption algorithm (default).
6. Saved the modified image as `ealao.bmp`.

**Observations**:
- No visible difference between `Lab1.bmp` and `ealao.bmp`.
- File sizes: identical or near-identical.
- Hidden data successfully extracted using the Reveal feature with correct passphrase.

📷 See **Slide 1 & 2** in the submission for screenshots and answers.

---

### 🔹 Part B: Invisible Secrets

**Steps Performed**:
1. Copied `plane.jpg` to `Ex5`.
2. Installed and launched Invisible Secrets.
3. Used “Hide Files” to embed an RTF with name and FOLID into `plane.jpg`.
4. Selected AES as the encryption algorithm with password `ism24`.
5. Output file: `emmanuel2.jpg`.

**Observations**:
- File size slightly increased, indicating data embedding.
- Successfully decrypted and extracted `Ex5 Stego.rtf`.

📷 See **Slides 3 & 4** for encryption options and extraction proof.

---

### 🔹 Part C: Xsteg/Stegdetect

**Steps Performed**:
1. Used `xsteg` tool to scan two images:
   - One unused image (e.g., `frigate.jpg`): No hidden content detected.
   - Embedded image (`emmanuel2.jpg`): Hidden data detected and identified the method used.

📷 See **Slide 5** for Xsteg output.

---

### 🔹 Part D: AES Crypt

**Steps Performed**:
1. Created `ealao.txt` with name, student ID, and FOL username.
2. Encrypted using command:

   ```bash
   aescrypt.exe -e -p info6001 ealao.txt
