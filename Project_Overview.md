Project Overview – PDF to Structured JSON Extraction
1. Introduction

The purpose of this project is to design and implement a program capable of parsing PDF documents and converting their contents into a structured JSON format. 
The solution aims to preserve the hierarchical organization of the document (pages, sections, sub-sections) while distinguishing between different data types 
such as paragraphs, bullet points, tables, and charts.

This project is useful for scenarios where PDF content must be transformed into a machine-readable format for further analysis, indexing, or integration with web applications.

2. System Architecture

The overall architecture of the system is illustrated below:

          ┌────────────────────┐
          │   Input PDF File   │
          └─────────┬──────────┘
                    │
                    ▼
        ┌───────────────────────────┐
        │   PDF Parsing Engine       │
        │ (pdfplumber + PyMuPDF +    │
        │  camelot)                  │
        └─────────┬─────────────────┘
                  │
  ┌───────────────┼───────────────────────┐
  │               │                       │
  ▼               ▼                       ▼
Text Extraction   Table Extraction        Image/Chart Detection
(pdfplumber)      (camelot/pdfplumber)    (PyMuPDF)
  │               │                       │
  └──────┬────────┴─────────┬─────────────┘
         │                  │
         ▼                  ▼
   Structure Analyzer   Format Cleaner
   (detects sections,   (remove spaces,
    sub-sections,        merge lines,
    bullets, etc.)       clean text)
         │
         ▼
 ┌───────────────────────────────┐
 │   JSON Builder                 │
 │  (organizes by page → section  │
 │   → sub-section → content)     │
 └───────────────┬───────────────┘
                 │
                 ▼
        ┌────────────────────┐
        │   Output JSON File │
        └────────────────────┘


3. Workflow Description
Step 1: Input

The user provides a PDF file as input. The file may contain plain text, structured sections
(e.g., resumes), or mixed data such as tables, images, and charts.

Step 2: Parsing the PDF

pdfplumber is employed for text and simple table extraction.

PyMuPDF (fitz) is used to access images, charts, and bounding box metadata.

camelot is used when advanced table parsing is required.

Step 3: Content Classification

Extracted content is classified according to structural rules:

Uppercase lines are treated as sections.

Lines containing : or | are treated as sub-sections.

Lines beginning with bullet characters (•, -, ●) are identified as bullet points.

All other lines are classified as paragraphs.

Step 4: Cleaning

Text is processed to remove extra whitespace, merge split lines, and normalize formatting for consistency.

Step 5: JSON Construction

The cleaned and structured content is organized hierarchically in JSON format. The hierarchy is as follows:

Page → Sections → Sub-sections → Content Blocks

Additional arrays store detected tables and charts/images.

Step 6: Output

The structured JSON is written to a file. The output can then be consumed by downstream applications,
search engines, or analytics pipelines.

4. Example JSON Output
{
  "pages": [
    {
      "page_number": 1,
      "sections": [
        {
          "section": "PROFILE",
          "sub_sections": [],
          "content": [
            {
              "type": "paragraph",
              "text": "Results-driven engineer with 2 years of experience..."
            },
            {
              "type": "bullet_point",
              "text": "Programming: Python, SQL, Power BI"
            }
          ]
        }
      ],
      "tables": [
        {
          "type": "table",
          "description": "Detected table",
          "table_data": [
            ["Year", "Revenue", "Profit"],
            ["2022", "$10M", "$2M"]
          ]
        }
      ],
      "charts": [
        {
          "type": "chart",
          "description": "Image/Chart detected",
          "bbox": [100, 200, 300, 400]
        }
      ]
    }
  ]
}

5. Limitations

Scanned PDFs (image-based) cannot be processed without OCR integration.

Documents with multi-column layouts may result in mis-ordered text extraction.

Complex or irregular tables may not be parsed accurately.

6. Future Enhancements

Integrate OCR (e.g., Tesseract) to handle scanned PDFs.

Employ machine learning–based layout analysis (e.g., LayoutLM, Detectron2) for improved section detection.

Apply natural language processing (NLP) for semantic enrichment, such as identifying named entities (names, dates, skills).

7. Conclusion

This project demonstrates an effective pipeline for converting PDF documents into structured JSON representations.
By combining multiple Python libraries (pdfplumber, PyMuPDF, and camelot), it handles diverse content types, 
preserves hierarchical structure, and produces output suitable for both human interpretation and machine consumption.
