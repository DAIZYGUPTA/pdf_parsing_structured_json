# 📄 PDF → Structured JSON Parser

##  Overview
This project parses a PDF file and converts it into a **structured JSON** format.  
It preserves the **page hierarchy**, detects **sections, sub-sections, paragraphs, bullet points, tables, and charts/images**, and organizes them in a machine-readable structure.

---

##  Requirements
- Python **3.8+**
- pip package manager
- (Optional for table extraction) **Ghostscript**

---

##  Dependencies & Installation

You can install all dependencies in one go using the `requirements.txt` file:

```bash
pip install -r requirements.txt
```

## Manual Installation (if needed)

1. Install Python libraries:

```bash
pip install pdfplumber
pip install PyMuPDF
pip install "camelot-py[cv]"
```
2. Install Ghostscript (needed for Camelot)
Camelot requires Ghostscript to parse tables.
Download and install Ghostscript from the official site:
 https://ghostscript.com/releases/gsdnld.html

On Windows, Ghostscript installs as gswin64c.exe. Add its path (e.g., C:\Program Files\gs\gs10.06.0\bin) to your system PATH.
On Linux/Mac, it installs as gs. It’s usually auto-available in PATH.

3. Verify Ghostscript is available:
```
   import subprocess

# Windows
print(subprocess.getoutput("gswin64c --version"))

# Linux/Mac
print(subprocess.getoutput("gs --version"))
```
If it prints a version number like 10.06.0, Ghostscript is correctly installed.

# How to Run the Parser

Run the script with:
```
python pdf_parser_to_json.py input.pdf output.json
```
input.pdf → path to your PDF file
output.json → path where extracted JSON will be saved








