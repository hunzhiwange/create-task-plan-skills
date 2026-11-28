---
name: pdf
description: Inspect, extract, create, merge, split, render, or fill PDF files. Use for text and tables, scanned pages, page ranges, and PDF forms.
---

# PDF

Start by checking the page count and whether the relevant pages have a usable text layer. Treat page numbers in the request as one-based. Libraries such as pypdf index pages from zero.

| Task | Approach |
|------|----------|
| Inspect text or metadata | Use `pdfinfo` and `pdftotext -layout input.pdf output.txt` when Poppler is available. Use `pypdf.PdfReader` for metadata or page-by-page text extraction in Python. |
| Extract tables | Open the relevant pages with pdfplumber. Compare the extracted rows with the rendered source, especially for merged headers and multi-line cells. |
| Read a scanned PDF | Render the relevant pages with Poppler and perform OCR with the appropriate language packs. Verify names, numbers, and totals against the page image. |
| Merge or split pages | Use `pypdf.PdfWriter.append` for whole files and `add_page` for explicit page selections. Confirm the final order and page count. |
| Fill form fields | Clone the source into a writer. Inspect the field names with `PdfReader.get_fields()` and fill the existing widgets with `PdfWriter.update_page_form_field_values`. A flat form needs an overlay placed against rendered page coordinates. |
| Create a PDF | Use ReportLab for programmatic layout or export from the authoring tool. Embed fonts supporting the output language. |

Use existing commands and Python libraries when available. If dependencies are missing, use the environment's normal package manager or a task-local Python environment. Keep the input PDF intact and use a separate output path.

Open the resulting PDF with a reader and verify the expected page count. Render representative pages and all edited form pages when layout matters. Check field appearances, clipping, margins, and font rendering, then return the PDF and any requested extracted files.
