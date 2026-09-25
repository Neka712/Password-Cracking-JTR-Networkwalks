# Password Cracking Projects — NetworkWalks Academy (Week 3)

**Name:** Omoneka Philomena Ekhaizor
**Program:** NetworkWalks Cybersecurity Internship

This report documents two related password-cracking exercises completed as part of the Week 3 practical module: one using **John the Ripper (via the Johnny GUI)**, and one using **NetworkWalks' own online Hash Calculator and Password Cracker tools**. Both target the same encrypted file, `My-Locked-PDF1.pdf`, and both successfully recovered the password and captured the flag.

---

# Project 1: Password Cracking with John the Ripper (Johnny GUI)

**Flag Captured:** `nw{networkwalks_flag1_jtr_270521_1}`

## 1. Background

This exercise involved recovering the password of a protected PDF file using **John the Ripper (JtR)**, a well-known password-cracking tool, paired with its GUI frontend, **Johnny**. The objective was to simulate a real-world offline password-cracking scenario against a document-level encryption hash, reinforcing concepts around hash extraction, dictionary/brute-force attacks, and password auditing.

## 2. Project Overview

The task required taking an encrypted PDF file (`My-Locked-PDF1.pdf`), extracting its password hash, feeding that hash into John the Ripper via the Johnny GUI, and running an attack until the password was recovered. The recovered password was then used to unlock the PDF, which contained a confirmation flag proving successful completion.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| **John the Ripper (Jumbo 1.9.0, Win64)** | Password-cracking engine |
| **Johnny GUI** | Graphical frontend for John the Ripper |
| **onlinehashcrack.com — PDF Hash Extractor** | Extracted the crackable hash from the PDF (pdf2john equivalent) |
| **Notepad** | Stored the extracted hash in a `.txt` file for John to read |
| **WPS Office PDF Viewer** | Used to verify the recovered password by opening the file |

## 4. Methodology

### Step 1 — Install John the Ripper and Johnny

Downloaded the John the Ripper Jumbo build and the Johnny GUI and installed them locally. On first launch, Johnny's Settings tab showed **"No valid John the Ripper executable detected"** since the path hadn't been configured yet.

![Johnny — no executable detected](screenshots/01-johnny-no-executable.png)

Using **Browse**, the path was set to:
```
...John_The_Ripper/john-1.9.0-jumbo-1-win64/run/john.exe
```
Johnny then confirmed detection: *"Detected John the Ripper 1.9.0-jumbo-1 OMP [cygwin 64-bit x86_64 AVX2 AC]"*.

![Johnny — executable detected](screenshots/02-johnny-executable-detected.png)

### Step 2 — Obtain the target file

Downloaded the encrypted target file, `My-Locked-PDF1.pdf`, to the local machine.

### Step 3 — Extract the password hash

Since John the Ripper cannot crack a PDF directly, its password protection first has to be converted into a crackable hash format. This is normally done locally with the `pdf2john.py` script bundled with JtR, but for this exercise the hash was extracted using the online tool **onlinehashcrack.com/tools-pdf-hash-extractor.php**, which performs the same conversion (`pdf2john`/`pdf2hashcat` equivalent).

The PDF was uploaded via **Browse → Upload**, which generated a hash string in the format:
```
$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a6...
```

![Online Hash Extractor tool output](screenshots/03-onlinehashcrack-tool.png)

### Step 4 — Save the hash for John the Ripper

The full hash string was copied and pasted into Notepad, then saved as `hash1.txt`. This text file is the input John the Ripper reads to identify and attack the hash.

![Hash saved in Notepad](screenshots/04-hash-in-notepad.png)

### Step 5 — Load the hash into Johnny

In Johnny, **Open password file** was used to browse to and load `hash1.txt`.

### Step 6 — Run the attack

**Start new attack** was clicked to begin cracking. Johnny passed the job to the John the Ripper engine in the background. The attack completed almost immediately, reaching **100% (1/1: 1 cracked, 0 left) [format=PDF]**.

### Step 7 — Review results

Switching to the **Passwords** tab in Johnny displayed the cracked credential:

| User | Password | Hash | Format |
|---|---|---|---|
| ? | `password1` | `$pdf$4*4*128*-...` | PDF |

![Johnny — Passwords tab showing cracked password](screenshots/05-johnny-passwords-cracked.png)

### Step 8 — Verify the password

The recovered password (`password1`) was entered into the "Input Open Password" prompt when reopening `My-Locked-PDF1.pdf` in WPS Office.

![PDF open-password prompt](screenshots/06-pdf-open-password-prompt.png)

### Step 9 — Confirm success

The PDF unlocked successfully, revealing a "Congratulations!" page confirming flag capture:

> **Flag1:** `nw{networkwalks_flag1_jtr_270521_1}`

![Congratulations page with captured flag](screenshots/07-congratulations-flag.png)

## 5. Results

- **Hash format identified:** PDF (revision 4, 128-bit key length)
- **Password recovered:** `password1`
- **Time to crack:** Near-instant — indicates the password was weak/dictionary-based rather than random
- **Attack mode:** John the Ripper's default mode (single crack → wordlist), which succeeded without needing a custom wordlist

## 6. Observations

- The speed of the crack (essentially instant) demonstrates how quickly weak, dictionary-style passwords (`password1`) fall to basic cracking tools — a good talking point on password policy weaknesses.
- Using a third-party website (onlinehashcrack.com) to extract the hash is convenient for a lab environment, but in a real engagement this would mean uploading potentially sensitive document data to an external server — the standard/safer practice is running `pdf2john.py` locally, which ships with the John the Ripper Jumbo package.
- Johnny is a convenience layer; all actual cracking happens through the John the Ripper CLI engine it wraps.

---

# Project 2: Password Cracking with NetworkWalks Tools (Hash Calculator + Password Cracker)

**Flag Captured:** `nw{networkwalks_flag1_jtr_270521_1}`

## 1. Background

This second exercise repeated the same password-recovery goal — cracking `My-Locked-PDF1.pdf` — but instead of using John the Ripper locally, it used **NetworkWalks Academy's own browser-based tools**: the **Hash Calculator** (to extract the PDF's crackable hash) and the **Password Cracker** (a dictionary-attack simulator that mimics how John the Ripper works). This demonstrates the same underlying concept of offline hash cracking, but through a simplified, web-based teaching tool.

## 2. Project Overview

**Task:** Crack the password of the attached PDF file (`My Locked PDF1.pdf`) using the NetworkWalks Hash Calculator and Password Cracker tools.

The workflow: download the encrypted PDF → extract its hash using the Hash Calculator → paste that hash into the Password Cracker → run the dictionary attack → recover the password → use it to open the PDF and capture the flag.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| **NetworkWalks Lab Task page** | Source of the encrypted PDF file(s) for the exercise |
| **NetworkWalks Hash Calculator** (`networkwalks.com/hash-calculator/`) | Extracted the crackable `$pdf$...` hash from the PDF, in-browser |
| **NetworkWalks Password Cracker** (`networkwalks.com/password-cracker/`) | Ran a dictionary attack against the extracted hash to recover the password |

## 4. Methodology

### Step 1 — Download the encrypted PDF

The encrypted file, `My Locked PDF1.pdf`, was downloaded from the NetworkWalks lab task page, which also listed two additional locked PDFs (`My Locked PDF2.pdf`, `My Locked PDF3.pdf`) and links to the two tools used in this exercise.

![NetworkWalks lab task page — file downloads and tool links](screenshots/08-nw-lab-task-page.png)

### Step 2 — Open the Hash Calculator

The NetworkWalks Hash Calculator was opened in the browser at `networkwalks.com/hash-calculator/`. The tool supports generating MD5, SHA-1, SHA-256, SHA-384, SHA-512 hashes from text, or extracting a crackable hash directly from a password-protected PDF.

![NetworkWalks Hash Calculator — tool opened](screenshots/09-nw-hash-calculator-opened.png)

### Step 3 — Upload the PDF and extract the hash

Switching to the **PDF** tab and uploading `My-Locked-PDF1.pdf`, the tool parsed the file locally in-browser and confirmed: *"This PDF is encrypted. A crackable hash has been extracted below (pdf2john / hashcat compatible format)."* The generated hash:

```
$pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
```

Details shown: **Revision: R4, Version: V4, Key length: 128 bit**.

![NetworkWalks Hash Calculator — extracted PDF hash](screenshots/10-nw-hash-calculator-result.png)

### Step 4 — Paste the hash into the Password Cracker

The full hash was copied and pasted into the **NetworkWalks Password Cracker** (`networkwalks.com/password-cracker/`), a tool described as: *"Hash every word in a wordlist and match it against a PDF password hash, the same idea John the Ripper uses."* The built-in wordlist (100 passwords) was left active.

![NetworkWalks Password Cracker — hash pasted, ready to run](screenshots/11-nw-password-cracker-hash-pasted.png)

### Step 5 — Run the attack

**Start Cracking** was clicked. The tool worked through the built-in wordlist at roughly 9 passwords/second, trying candidates such as `service`, `canada`, `hockey`, `killer`, `george`, `asdfgh`, `zxcvbn`, `qwertyuiop`, and `111222` before finding a match at attempt 91 of 100.

### Step 6 — Review results

The tool displayed:

> **PASSWORD CRACKED SUCCESSFULLY**
> `password1`
> *This is the PDF password. Open your PDF and enter it to unlock the file.*

![NetworkWalks Password Cracker — password cracked](screenshots/12-nw-password-cracker-cracked.png)

### Step 7 — Open and verify the PDF

`My-Locked-PDF1.pdf` was opened in the browser, which prompted: *"This file is password protected. Please enter a password to open the file."*

![PDF — enter password prompt](screenshots/13-nw-pdf-enter-password-prompt.png)

Entering `password1` unlocked the file successfully.

### Step 8 — Confirm success

The PDF opened to a "Congratulations!" page confirming flag capture:

> **Flag1:** `nw{networkwalks_flag1_jtr_270521_1}`

![Congratulations page with captured flag](screenshots/14-nw-congratulations-flag.png)

## 5. Results

- **Hash format identified:** PDF (revision 4, version 4, 128-bit key length) — identical hash to Project 1, confirming the same underlying encryption
- **Password recovered:** `password1`
- **Attempts required:** 91 out of 100 words in the built-in wordlist
- **Speed:** ~9 passwords/second (browser-based simulation, intentionally slower/visual for teaching purposes compared to John the Ripper's native speed)

## 6. Observations

- The NetworkWalks Password Cracker is a simplified, visual teaching tool that mimics John the Ripper's dictionary-attack logic in-browser — useful for understanding the concept without needing local software installed.
- Both projects arrived at the identical password (`password1`) for the identical hash, cross-confirming the result from Project 1.
- Because the hash extraction happens client-side ("parsed locally," "no text or file is ever uploaded" per the Hash Calculator's own notes), this tool is arguably more privacy-conscious for a lab setting than routing the file through a third-party site.

---

## Overall Conclusion

Both projects targeted the same password-protected file and reached the same result — the password `password1` — using two different toolchains: a professional, locally-installed cracking suite (John the Ripper + Johnny) versus a lightweight, browser-based teaching tool (NetworkWalks Hash Calculator + Password Cracker). Together, they reinforce the same core lesson: **weak, predictable passwords are cracked almost immediately**, whether by industry-standard tools or simplified dictionary-attack simulators. This highlights the importance of strong password policies in real-world environments.
