# PDF Reader MCP Server

A Model Context Protocol (MCP) server that provides tools for reading and extracting text from PDF files, supporting both local files and URLs.

## Features

- Read text content from local PDF files
- Read text content from PDF URLs
- Error handling for corrupt or invalid PDFs
- Volume mounting for accessing local PDFs
- Auto-detection of PDF encoding
- Standardized JSON output format

## Installation

1. Clone the repository:

```bash
git clone http://github.com/Frallen/pdf-mcp
cd pdf-mcp
```

2. Build the Docker image:

```bash
docker build -t mcp/pdf-mcp .
```

## Usage

### Running the Server

To run the server with access to local PDF files:

```bash
docker run -i --rm -v /path/to/pdfs:/pdfs mcp/pdf-mcp
```

Replace `/path/to/pdfs` with the actual path to your PDF files directory.

If not using local PDF files:

```bash
docker run -i --rm mcp/pdf-mcp
```

### MCP Configuration

Add to your MCP settings configuration:

```json
{
  "mcpServers": {
    "pdf-mcp": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-v",
        "/path/to/pdfs",
        "mcp/pdf-mcp"
      ],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Without local file PDF files:

```json
{
  "mcpServers": {
    "pdf-mcp": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "mcp/pdf-mcp"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

### Available Tools

1. `read_local_pdf`

   - Purpose: Read text content from a local PDF file
   - Input:
     ```json
     {
       "path": "/pdfs/document.pdf"
     }
     ```
   - Output:
     ```json
     {
       "success": true,
       "data": {
         "text": "Extracted content..."
       }
     }
     ```

2. `read_pdf_url`
   - Purpose: Read text content from a PDF URL
   - Input:
     ```json
     {
       "url": "https://example.com/document.pdf"
     }
     ```
   - Output:
     ```json
     {
       "success": true,
       "data": {
         "text": "Extracted content..."
       }
     }
     ```

## Error Handling

The server handles various error cases with clear error messages:

- Invalid or corrupt PDF files
- Missing files
- Failed URL requests
- Permission issues
- Network connectivity problems

Error responses follow the format:

```json
{
  "success": false,
  "error": "Detailed error message"
}
```

## Dependencies

- Python 3.11+
- PyPDF2: PDF parsing and text extraction
- requests: HTTP client for fetching PDFs from URLs
- MCP SDK: Model Context Protocol implementation

## Project Structure

```
.
├── Dockerfile          # Container configuration
├── README.md          # This documentation
├── requirements.txt   # Python dependencies
└── src/
    ├── __init__.py    # Package initialization
    └── server.py      # Main server implementation
```