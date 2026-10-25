# static.md for Claude

[static.md](https://static.md) hosts markdown notes and Excalidraw whiteboards that you can share with a link and edit together in real time. This plugin connects Claude to your static.md account, so Claude can publish what you write together as a shareable note, sketch diagrams on a whiteboard, and keep your existing notes up to date.

## What you can use it for

- **Memory shared between agents.** Claude Code writes decisions and a handoff note while it works; tomorrow claude.ai on your phone, Cursor or another agent reads the same note and carries on.
- **Todo lists and plans Claude keeps current.** Ask Claude to plan your week or a project, and it keeps the note or board up to date as things change. Open it on any device.
- **Notes during a call.** Everyone on the call opens the same note link and types together with live cursors, while Claude adds the summary, decisions and action items.
- **Shareable answers.** "Put this on static.md" turns a summary, a spec or meeting notes into a link you can send, instead of a wall of text in chat.
- **Diagrams you can keep editing.** Claude sketches an architecture, a flow or a timeline on an Excalidraw whiteboard; you and your team refine it by hand.
- **A notepad between you and your assistant.** One note with a dated section Claude writes into, and you write requests back from your phone.

## What's inside

- **The static.md MCP server** at `https://static.md/mcp`. It gives Claude 17 tools:
  - notes: `list_my_notes`, `read_note`, `create_note`, `update_note`, and `search_notes`, which finds notes by the words in their title or text (Pro);
  - whiteboards: `list_my_boards`, `read_board`, `create_board`;
  - folders: `move_item`, which renames a note or whiteboard or moves it to another folder;
  - sharing: `set_access` and `set_read_only`, which change who can open or edit a note or whiteboard, only when you ask;
  - images: `upload_image`, which puts a screenshot or chart on static.md and returns a snippet to embed in a note;
  - memory: `memory_view` and `memory`, Anthropic's memory commands over files in your `/memories` folder, each one a note you can open;
  - end-to-end encrypted notes (Pro): `read_note_ciphertext`, `create_note_ciphertext` and `write_note_ciphertext`, which carry only ciphertext. An agent on your computer reads and writes the text through the open-source [E2E bridge](https://github.com/static-md/e2e-bridge), which keeps the key on your side.

  Notes can link to each other with `[[wikilinks]]`: `read_note` lists the notes a note links to and the notes that link back to it.
- **Three skills** that teach Claude when and how to use those tools:
  - `share-as-note`: publishes markdown from the conversation as a note and returns its link;
  - `sketch-whiteboard`: draws a diagram on a new whiteboard and returns its link;
  - `edit-notes`: finds, reads and updates your existing notes.

## Sign-in and permissions

The first time Claude uses static.md, it asks you to sign in to static.md (with Google, GitHub or email) and shows what Claude will be able to do. You approve reading and editing notes and whiteboards and uploading images. Changing who can open them is a separate permission that you approve on its own. On Pro you can also limit Claude to some of your folders. You can disconnect Claude at any time at [static.md/connected-apps](https://static.md/connected-apps).

## What it sends where

The plugin runs no code on your machine. Claude talks to one service, static.md's MCP server at `https://static.md/mcp`, over HTTPS. It sends:

- the notes and whiteboards you ask it to create or change;
- the images you ask it to upload;
- the ids of the items it reads and the words it searches for.

Everything Claude reads comes from your static.md account. The plugin alone can't read or change end-to-end encrypted notes and whiteboards: static.md never has their key. On Pro, the optional E2E bridge, run on your computer, can read and write encrypted notes for an agent there.

New notes and whiteboards open to anyone with their link, like ones you make on static.md. Claude changes who can open them only when you ask.

## Plans

On the free plan, agents can create 10 notes or whiteboards per hour and make 50 tool calls a day, and whiteboards hold up to 300 elements. [Pro](https://static.md/pricing) raises the limit to 5,000 tool calls a day, removes the others, and adds search, restricted sharing, read-only notes, end-to-end encryption, limiting an agent to some folders, and a 30-day history of agent edits that you can restore.

## Links

- Setup for other AI apps: [static.md/agents](https://static.md/agents)
- [Terms of Service](https://static.md/terms) and [Privacy Policy](https://static.md/privacy)
- Support: [salut@static.md](mailto:salut@static.md)
