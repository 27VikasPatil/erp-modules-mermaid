# ERP Modules Mermaid Diagrams

This repository contains Mermaid markdown files for all 18 ERP modules, plus consolidated diagrams and instructions.

## How to Render and Export Mermaid Diagrams

### Option 1: Online Mermaid Live Editor

1. Go to [Mermaid Live Editor](https://mermaid.live).
2. Copy the code block from any `.md` file in this repo.
3. Paste into the editor, view the rendered diagram.
4. Use the **Download** button to export as PNG or SVG.
5. Combine exported images into a PDF as needed.

### Option 2: Local CLI (Advanced)

- Install Node.js and [Mermaid CLI (mmdc)](https://github.com/mermaid-js/mermaid-cli).
- Save each diagram code block as a `.mmd` file.
- Run:  
  `mmdc -i diagram.mmd -o diagram.png`
- Use PDF tools to combine images or convert to PDF.

### Option 3: Markdown Viewer with Mermaid Support

- Use VS Code with the "Markdown Preview Mermaid Support" extension.
- Open the markdown file, preview Mermaid diagrams.
- Use screenshot or export options.

## Files in this Repo

- `module1-marketing-management.md` ... `module18-administration-management.md`
- `master-consolidated-flowchart.md` (Overall flow)
- `cross-module-integration-diagram.md` (Integration points)
- `role-swimlane-overlay.md` (Role overview)
- `README.md` (This file)

## Tips

- You can zip all files using GitHub's "Download ZIP" feature.
- For PDF export, use free online converters or combine exported images.
- Mermaid diagrams are text-based and easy to edit for customization.

---

_If you need more instructions or custom visuals, contact the repo owner!_