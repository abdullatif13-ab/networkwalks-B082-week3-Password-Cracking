# NetworkWalks B082 — Week 3 · Password Cracking

<p align="center">
  <img src="https://img.shields.io/badge/purpose-educational%20only-blue" alt="Educational">
  <img src="https://img.shields.io/badge/testing-authorized-success" alt="Authorized">
  <img src="https://img.shields.io/badge/tool-John%20the%20Ripper-red" alt="John the Ripper">
  <img src="https://img.shields.io/badge/tool-Johnny%20GUI-orange" alt="Johnny">
  <img src="https://img.shields.io/badge/tool-NetworkWalks%20Online%20Tools-purple" alt="NetworkWalks Tools">
  <img src="https://img.shields.io/badge/NetworkWalks-Week%203-red" alt="Week 3">
</p>

This repo documents **Week 3** of my Cybersecurity & Ethical Hacking internship with
NetworkWalks Academy (Batch B082). Week 3 is all about **password cracking** — recovering
the passwords of encrypted PDF files and confirming the results by opening them. It has
two parts, each using a different toolset for the same goal:

- **PM1 — Password Cracking with JTR:** using **John the Ripper (JTR)** and its graphical
  front-end **Johnny** — the industry-standard cracking tools.
- **PM2 — Password Cracking with NetworkWalks Tools:** using NetworkWalks' own free,
  browser-based **Hash Calculator** and **Password Cracker** — no installation needed.

Both modules follow the same idea: take the password hash out of a locked PDF, then run a
dictionary attack that tries word after word until one matches. Doing it two ways shows
that a professional CLI tool and a simple web tool rely on the **exact same underlying
technique** — and that a weak password falls to both in seconds.

---

## ⚠️ Authorization & Scope

All files cracked here are **practice PDFs provided by NetworkWalks** as part of the B082
internship — they are deliberately locked as a training (capture-the-flag) exercise.

**In scope:**
- The course-provided practice PDFs (`My Locked PDF1.pdf`, `PDF2`, `PDF3`), cracked on my
  own lab machine.

Nothing else was tested — no third-party files and no unauthorized systems, only the
training material I was given, on hardware I own and control.

## PM1 · Password Cracking with John the Ripper (JTR)

**John the Ripper (JTR)** is the industry-standard password cracker; **Johnny** is its
point-and-click GUI. The goal of this module was to recover the passwords of three locked
practice PDFs and open them to confirm.

### Tools
- **John the Ripper (jumbo)** — command-line cracker
- **Johnny** — JTR graphical front-end
- **Online PDF Hash Extractor (pdf2john)** — https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php

### Steps

**1. Set up and verify John (CLI)**

Confirm John works and supports PDF cracking — a `PASS` on the PDF self-test means it's ready:

```bash
john --list=build-info
john --test=0 --format=PDF
```

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_1_John_the_Ripper_CLI_Setup.png" width="750">
</p>
<p align="center"><i>John built and verified — PDF format self-test returns PASS.</i></p>

**2. Open Johnny (GUI)**

Johnny runs John underneath, so it must be pointed at a valid John executable. Once detected,
it reports the John version and is ready to attack.

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_2_Johnny_GUI_Ready.png" width="750">
</p>
<p align="center"><i>Johnny detects John the Ripper 1.9.0-jumbo — ready for attacks.</i></p>

**3. Extract the PDF password hash**

Upload each locked PDF to the online extractor to get its hash. The output **must start with
`$pdf$`** — if it begins with `b'`, remove that. Save each hash to its own file
(`hash1.txt`, `hash2.txt`, `hash3.txt`).

- https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_3_Extracting_the_PDF_Hash.png" width="750">
</p>
<p align="center"><i>The locked PDF converted to its <code>$pdf$</code> hash.</i></p>

**4. Crack the hashes**

Cracking was done in Johnny (*Open password file → Start new attack*). Johnny runs John
underneath — the equivalent CLI commands are:

```bash
john hash1.txt          # start cracking
john --show hash1.txt   # show the recovered password
```

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_4_Cracking_PDF_1_good_luck.png" width="750">
</p>
<p align="center"><i>PDF 1 cracked → <b>good-luck</b>.</i></p>

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_5_Cracking_PDF_2_password1.png" width="750">
</p>
<p align="center"><i>PDF 2 cracked → <b>password1</b>.</i></p>

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_6_Cracking_PDF_3_1qaz2wsx.png" width="750">
</p>
<p align="center"><i>PDF 3 cracked → <b>1qaz2wsx</b>.</i></p>

**5. Verify — open the unlocked PDFs**

Each recovered password opened its PDF, confirming the crack. PDF 3 revealed the flag.

<p align="center">
  <img src="W3-PM1-JTR/screenshots/Step_7_All_Three_PDFs_Unlocked.png" width="800">
</p>
<p align="center"><i>All three PDFs unlocked with their recovered passwords.</i></p>

### Results — PM1

| PDF file            | Recovered password | Why it was weak                  |
|---------------------|--------------------|----------------------------------|
| My Locked PDF1.pdf  | `good-luck`        | short dictionary phrase          |
| My Locked PDF2.pdf  | `password1`        | one of the most common passwords |
| My Locked PDF3.pdf  | `1qaz2wsx`         | keyboard-walk pattern            |

**Flag captured (PDF 3):** `nw{networkwalks_flag_260821_1}`
