# ATARS 5.4.7 — the certificate

Every saved analysis can now produce a **Certificate of Verified Analysis**: one page
that states what was in the file, what was done to it, and whether the numbers still
match the data. Each sheet gets a signed QR code, and ATARS can read those codes back
— from the PDF, or from a photo of a printed page — and check them against the data
itself. A certificate that has been edited stops verifying.

## What's new

- **Certificate of Verified Analysis** for the whole saved workbook, carrying the
  ATARS logo and a signed QR code for every sheet
- **Verify from a photo** — point ATARS at a picture of a printed certificate and it
  reads the codes and re-checks them
- True/False columns **no longer crash the report**
- `.pkl` files **open again**, through a loader that refuses unsafe pickles
- Nested JSON now loads as **the table inside it**, not a single unusable column
- **Save as** keeps every sheet, including the empty ones

## Download

| | |
|---|---|
| **Microsoft Store** (recommended) | https://apps.microsoft.com/detail/9NFRBPL4T2M8 |
| **Direct installer** | `ATARS-Setup.exe` below — 350.4 MB |

Windows 10 / 11, 64-bit. Per-user install, no admin rights. About 1.2 GB once installed.

### Verify your download

```
sha256  1871ec7b4d2eb6c9bb44f80dd756cb8bf1374786b71bd7131eee2bb2c2b8816b
```

In PowerShell or Command Prompt, from the folder you downloaded it to:

```
certutil -hashfile ATARS-Setup.exe SHA256
```

The line it prints must match the hash above, character for character.

### About the Windows warning

The direct installer is not code-signed, so Windows shows **"Windows protected your
PC"**. That is a notice about the absence of a paid certificate, not a virus warning.
Click **More info → Run anyway**, or install from the Microsoft Store instead, where
Microsoft signs the package and no warning appears.

## Your data

ATARS runs entirely on your own computer. Files you open are not uploaded anywhere.
AI features are off until you add your own key, and work without one.
