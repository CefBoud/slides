# MCP Elasticsearch Slides

This folder contains slides for MCP and Elasticsearch integration.

## Generating Slides

To generate and view the slides from the markdown file:

```bash
npx @slidev/cli mcp-elasticsearch-slides.md
```

### Prerequisites

- Node.js >= 18

### Exporting Slides

To export the slides to different formats, first install the required dependency:

```bash
npm i -D playwright-chromium
```

Then, run:

- Export to PDF (default):

  ```bash
  npx @slidev/cli export mcp-elasticsearch-slides.md 
  ```

- Export to PPTX:

  ```bash
  npx @slidev/cli export mcp-elasticsearch-slides.md  --format pptx
  ```
