---
name: build-document
description: Build, develop, and maintain the CARTA frontend documentation website and API docs.
---

# Build Document Skill

This skill provides guidelines and commands for building, developing, and versioning the CARTA frontend documentation website, which is hosted on GitHub Pages. The documentation website is built with Docusaurus and API is based on the plugin `@loveluthien/docusaurus-plugin-typedoc-api`.

## 1. Local Development and Building

The documentation website is located in the `docs_website/` directory. Use the following commands to develop or build the site locally.

**Commands (run from within `docs_website/`):**
- Install dependencies: `npm install`
- Start development server with auto-reload: `npm start`
- Create a production build: `npm run build`
- Test the production build locally: `npm run serve`

*Note: The search feature is only available in production builds.*

## 2. Formatting

We use Prettier to maintain consistent markdown styling (indentation, line length, list numbering). 

**Commands (run from within `docs_website/`):**
- Check markdown formatting: `npm run checkformat`
- Auto-fix formatting issues: `npm run reformat`

## 3. Writing Documentation Pages

When creating or modifying documentation pages:
- **Location:** General documentation pages are located in the `docs_website/docs/` directory, while the API overview page is in the `docs_website/api/` directory.
- **Editing:** Edit markdown files directly to modify content or add new pages.
- **MDX:** Use the `.mdx` extension if you are using MDX components.
- **Version-Specific Links:** To link across versioned documentation, use specific components rather than standard markdown links:
  - Use the `DocsIndexLink` component for Docs index pages.
  - Use the `ApiLink` component for API subpages.

## 4. Writing API Documentation

API subpages are auto-generated from TSDoc comments in the codebase.
- **Export Requirements:** Catalogs are based on `index.ts` files. Elements must be explicitly exported in their respective `index.ts` to appear in the catalogs.
- **Visibility:** Private and protected elements are not displayed in the generated documentation.
- **Rebuilding:** The development server does not automatically rebuild TSDoc. A manual rebuild is required after changing TSDoc comments.

**TSDoc Format:**
- Follow the official [TSDoc documentation](https://tsdoc.org/).
- Format requirements are enforced by ESLint.
- To check TSDoc formatting, run `npm run check-eslint` from the **repository root**.

## 5. Versioning

To tag a new documentation version, run these commands from within `docs_website/`:

```bash
npm run docusaurus docs:version 1.2.3
npm run docusaurus api:version 1.2.3
```

*This process automatically updates `versions.json` and creates versioned snapshots in the `versioned_docs/` and `versioned_sidebars/` folders.*