# static.md

**Markdown notes your AI agents can read and write.**

[static.md](https://static.md) is collaborative markdown notes and whiteboards with a built-in MCP server, so Claude, Cursor, VS Code and other agents can work in the same documents you do. It also does free image hosting.

[Try the editor](https://static.md/md/demo) · [Connect your agent](https://static.md/agents) · [Pricing](https://static.md/pricing)

> **About this repository:** this is the open-source **image-hosting core** of static.md (uploads, galleries, resizing). Notes, whiteboards and the MCP server are hosted features at [static.md](https://static.md), and their code isn't in this repository.

## Connect your AI agent

The MCP server lives at `https://static.md/mcp` (Streamable HTTP). You sign in once with your static.md account, through OAuth.

**Claude Code**

```bash
claude mcp add --transport http static-md https://static.md/mcp
```

**Cursor**

[![Add static.md MCP server to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/link/mcp/install?name=static-md&config=eyJ1cmwiOiJodHRwczovL3N0YXRpYy5tZC9tY3AifQ==)

**VS Code**

[Install in VS Code](https://insiders.vscode.dev/redirect/mcp/install?name=static-md&config=%7B%22type%22:%22http%22%2C%22url%22:%22https://static.md/mcp%22%7D)

Setup for claude.ai, Claude Desktop and other clients is on [static.md/agents](https://static.md/agents).

## What static.md does

At [static.md](https://static.md):

- **Collaborative markdown notes.** Edit, split or preview mode with live multi-user editing, syntax highlighting, Mermaid diagrams and task lists. No account needed to start.
- **Whiteboards.** Excalidraw boards that sync in real time and can be embedded in notes.
- **MCP server for AI agents.** With your approval, agents can list, read, create and update your notes, read and create your whiteboards, and change who can open them. Note content reaches the agent marked as untrusted data.
- **End-to-end encryption (Pro).** Encrypted notes are encrypted in your browser, so neither static.md nor any agent can read them.
- **Sharing controls (Pro).** Read-only links, and access limited to the email addresses you list.
- **Image hosting (this repository).** Drag-and-drop uploads with short links, multi-image galleries, on-the-fly resizing with `?size=N`, served from a CDN.
- **Screenshots.** The [StaticShot Chrome extension](https://chromewebstore.google.com/detail/staticshot-screenshot-cap/bbgoenllpdnfljjapjcababahphohncj) and [StaticShot for Mac](https://static.md/mac) capture, annotate and upload in one step.

Questions or feedback: [salut@static.md](mailto:salut@static.md).

---

## The image-hosting core

> This project started in 2013 as a personal image hosting tool. This is the code running since 2016, slightly adjusted to run on Firebase. It was originally built with Express, MongoDB, and Pug, and ran on a single DigitalOcean droplet for over a decade. In 2026 it was migrated to Firebase (Hosting + Cloud Functions + Firestore + Cloud Storage) and the source code was made public.

### Architecture

- **Firebase Hosting**: CDN, SSL and SPA rewrites
- **Cloud Functions**: Express API (`api`) and image serving (`imageHandler`)
- **Firestore**: photos, galleries, upload tokens and link lookups
- **Cloud Storage**: image file storage (`uploads/`)

### Image serving flow

```
GET https://static.md/{hash}.jpg
  → Firebase CDN (cache hit? serve immediately)
  → imageHandler Cloud Function
    → Firestore lookup (links → photos)
    → Stream from Cloud Storage (or Sharp resize if ?size=N)
    → Cache-Control: s-maxage=315360000 (CDN caches for 10 years)
```

### API endpoints

| Method | Path | Description |
|--------|------|-------------|
| POST | `/api/v2/get-token` | Get time-limited upload token (MD5 challenge) |
| POST | `/api/v2/upload` | Upload with token |
| POST | `/api/v4/upload` | Upload + auto-create gallery |
| GET | `/api/v4/g/:id` | Get gallery photos |

### Development

```bash
# Frontend (Vite dev server)
npm install
npm run dev

# Functions
cd functions && npm install && npm run build

# Deploy everything
npm run deploy

# Deploy only functions or hosting
npm run deploy:functions
npm run deploy:hosting
```

### Project structure

```
├── src/                  # Vue 3 frontend
├── hosting/              # Built frontend (Vite output)
├── functions/src/        # Cloud Functions
│   ├── index.ts          # Function exports (api + imageHandler)
│   ├── imageHandler.ts   # Image serving + resize
│   ├── v2.ts             # V2 token-based upload API
│   ├── v4.ts             # V4 upload + gallery API
│   ├── photoTools.ts     # Upload logic, MD5 dedup, validation
│   ├── parseForm.ts      # Multipart form parsing (busboy)
│   └── utils.ts          # Random ID generation, helpers
├── migration/            # One-time MongoDB → Firestore migration
├── firebase.json         # Hosting rewrites, function config
├── firestore.rules       # Deny all client access
└── storage.rules         # Public read, admin write
```
