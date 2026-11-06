---
name: convert-documents-to-markdown
description: Convert documents (Word, Excel, PPT, PDF, etc.) to Markdown format using anydoc CLI. Invoke when user wants to convert documents to Markdown, extract text/content from documents, or transform document files into readable Markdown format.
---

# Convert Documents to Markdown

## Description

This skill converts various document formats (Word, Excel, PPT, PDF, HTML, etc.) to clean Markdown format using the anydoc CLI tool from @firecrawl/anydoc.

## When to Use

Invoke this skill when:
- User wants to convert a document to Markdown
- User needs to extract content from a document in Markdown format
- User mentions converting Word/Excel/PPT/PDF to Markdown
- User wants to make document content readable as text

## Supported Formats

- Word: .docx, .doc
- Excel: .xlsx, .xls, .csv
- PowerPoint: .pptx, .ppt
- PDF: .pdf
- HTML: .html, .htm
- Other: .txt, .rtf, .epub

## Prerequisites

Before using this skill, ensure Node.js is available. The anydoc CLI runs via npx, so it requires Node.js and npm.

To check:

    node --version
    npm --version

If Node.js is not installed, guide the user to install it first.

## Instructions

### Step 1: Verify the input file

1. Confirm the file exists at the given path
2. Check the file extension to determine the format
3. If the file format is unsupported, inform the user and suggest alternatives

### Step 2: Run the conversion

Use anydoc CLI via npx to convert the document:

    npx @firecrawl/anydoc convert <input_file_path>

For example:

    npx @firecrawl/anydoc convert C:/Users/Rational/Documents/report.docx

The converted Markdown will be output to the terminal. You can also save it to a file:

    npx @firecrawl/anydoc convert <input_file_path> -o <output_md_path>

### Step 3: Present the result

1. Display the converted Markdown content to the user
2. If the output was saved to a file, provide the file path
3. Inform the user about any formatting details or limitations

## Important Notes

- The first run of npx @firecrawl/anydoc will download the package automatically
- For large documents, the conversion may take some time
- Complex formatting (tables, images, layouts) may not convert perfectly
- Images in documents will be referenced but not extracted
- If the conversion fails, check if the file is corrupted or password-protected

## Error Handling

If the conversion fails:
1. Verify the file path is correct
2. Check if the file is password-protected or corrupted
3. Try converting a simpler document first
4. If anydoc doesn't support the format, suggest alternative tools

## Example

User says: Convert this Word document to Markdown: C:/Users/Rational/Documents/report.docx

Action:
1. Verify the file exists
2. Run: npx @firecrawl/anydoc convert "C:/Users/Rational/Documents/report.docx" -o "C:/Users/Rational/Documents/report.md"
3. Read and present the output Markdown file
4. Show the user the result and note any formatting issues
