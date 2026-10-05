# static.md for Claude

[static.md](https://static.md) hosts markdown notes and Excalidraw whiteboards that you can share with a link and edit together in real time. This plugin connects Claude to your static.md account, so Claude can publish what you write together as a shareable note, sketch diagrams on a whiteboard, and keep your existing notes up to date.

## What's inside

- **The static.md MCP server** at `https://static.md/mcp`. It gives Claude eight tools:
  - notes: `list_my_notes`, `read_note`, `create_note`, `update_note`;
  - whiteboards: `list_my_boards`, `read_board`, `create_board`;
  - sharing: `set_access`, which changes who can open a note or whiteboard.
- **Three skills** that teach Claude when and how to use those tools:
  - `share-as-note`: publishes markdown from the conversation as a note and returns its link;
  - `sketch-whiteboard`: draws a diagram on a new whiteboard and returns its link;
  - `edit-notes`: finds, reads and updates your existing notes.

## Sign-in and permissions

The first time Claude uses static.md, it asks you to sign in to static.md with Google and shows what Claude will be able to do. You approve reading and editing notes and whiteboards. Changing who can open them is a separate permission that you approve on its own. You can disconnect Claude at any time at [static.md/connected-apps](https://static.md/connected-apps).

## What it sends where

The plugin runs no code on your machine. Claude talks to one service, static.md's MCP server at `https://static.md/mcp`, over HTTPS. It sends:

- the notes and whiteboards you ask it to create or change;
- the ids of the items it reads.

Everything Claude reads comes from your static.md account. End-to-end encrypted notes and whiteboards can't be read or changed through the plugin.

New notes and whiteboards open to anyone with their link, like ones you make on static.md. Claude changes who can open them only when you ask.

## Plans

On the free plan, agents can create 10 notes or whiteboards per hour and make 50 tool calls a day, and whiteboards hold up to 300 elements. [Pro](https://static.md/pricing) removes those limits and adds restricted sharing and end-to-end encryption.

## Links

- Setup for other AI apps: [static.md/agents](https://static.md/agents)
- [Terms of Service](https://static.md/terms) and [Privacy Policy](https://static.md/privacy)
- Support: [salut@static.md](mailto:salut@static.md)
