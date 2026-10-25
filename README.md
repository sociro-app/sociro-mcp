# Sociro MCP server

**Post to your social accounts by asking your AI assistant.**

The Sociro MCP server connects Claude, ChatGPT, Cursor, VS Code, Gemini CLI, Windsurf or any app that speaks the
[Model Context Protocol](https://modelcontextprotocol.io) to your [Sociro](https://sociro.com) account. Ask in plain
words and your assistant lists your brands and accounts, adds an image, gives each platform its own caption, saves a
draft, schedules or publishes a post, tells you what is scheduled, and edits or cancels it. It can also plan a run of
posts with Sociro's AI Planner and approve them. One connection covers every brand in your workspace.

```
https://mcp.sociro.com/mcp
```

This is the official server, hosted by Sociro. There is nothing to install: add the address to your assistant and
sign in. This repository holds its documentation; the server itself runs at the address above.

- Website and full guides: [sociro.com/mcp](https://sociro.com/mcp)
- Status: beta (version 0.8.0)

## What you need

- A Sociro account on a paid plan ([pricing](https://sociro.com/pricing))
- To be the workspace owner or an admin, to approve the connection
- Or, for an app that cannot sign in: an API key from **Settings > Developer API** in Sociro

## Connect

Every app signs in the same way: your browser opens Sociro, you choose the workspace, every brand or one brand, and
what the assistant may do (see your accounts and posts, schedule posts, add media), then choose **Allow**.
Then ask: **"List my Sociro accounts."**

### Claude (web and Desktop)

Settings > Connectors > **Add custom connector**. Name it Sociro, paste the address, leave everything else empty and
connect. Full guide: [sociro.com/mcp/claude](https://sociro.com/mcp/claude)

### ChatGPT

Turn on **Developer mode** in Settings (under Connectors > Advanced, or Security; on a business plan your admin may
need to turn it on). In Connectors choose **Create**, paste the address, set Authentication to **OAuth**, confirm you
trust the app, and create. Turn the connector on in a new chat. Full guide: [sociro.com/mcp/chatgpt](https://sociro.com/mcp/chatgpt)

### Cursor

`~/.cursor/mcp.json` (every project) or `.cursor/mcp.json` (one project):

```json
{
  "mcpServers": {
    "sociro": {
      "url": "https://mcp.sociro.com/mcp"
    }
  }
}
```

Open the MCP servers list, check Sociro is on, and sign in when asked. Full guide: [sociro.com/mcp/cursor](https://sociro.com/mcp/cursor)

### VS Code with GitHub Copilot

Run **MCP: Add Server**, or add to `.vscode/mcp.json` (or **MCP: Open User Configuration** for every project):

```json
{
  "servers": {
    "sociro": {
      "type": "http",
      "url": "https://mcp.sociro.com/mcp"
    }
  }
}
```

Run **MCP: List Servers**, start sociro and sign in. Full guide: [sociro.com/mcp/vscode](https://sociro.com/mcp/vscode)

### Gemini CLI

```bash
gemini mcp add --transport http sociro https://mcp.sociro.com/mcp
```

Add `-s user` to have it in every project. Then in Gemini CLI run `/mcp auth sociro` to sign in, and `/mcp` to check
it is connected. Full guide: [sociro.com/mcp/gemini](https://sociro.com/mcp/gemini)

### Windsurf

Add the server address in Windsurf's MCP settings (`~/.codeium/windsurf/mcp_config.json`):

```json
{
  "mcpServers": {
    "sociro": {
      "serverUrl": "https://mcp.sociro.com/mcp"
    }
  }
}
```

### Apps that cannot sign in

Add the same address as a custom connector, choose no sign-in, and add one request header:

```
Authorization: Bearer <your Sociro API key>
```

The key comes from **Settings > Developer API** in Sociro. Never commit it to a repository.

## Tools

| Tool | What it does |
|---|---|
| `list_accounts` | List your brands (social groups) and the accounts in each |
| `get_platform_rules` | See what each platform accepts, so a post fits the first time |
| `create_post` | Schedule or publish a post, or save it as a draft |
| `get_post` | Get one post, with public links once published |
| `list_posts` | List posts: scheduled, published, failed or cancelled |
| `update_post` | Edit a scheduled post or a draft: caption, media, accounts or time |
| `cancel_post` | Cancel a scheduled post or a draft |
| `publish_draft` | Publish or schedule a draft |
| `import_image` | Add an image from a link |
| `set_video_thumbnail` | Set a video's thumbnail |
| `list_media` | Find a file already in your media library |
| `get_account_insights` | How a Facebook, Instagram or Threads account is doing: followers, the week's numbers, the latest posts |
| `get_posting_activity` | What was posted and what failed, per platform, account and day |
| `get_brand` | Read the brand a plan is written for |
| `draft_brand` | Draft a brand from a website |
| `save_brand` | Save a brand |
| `plan_posts` | Plan a run of posts with Sociro's AI Planner, saved for your review |
| `list_plan_posts` | List posts waiting for review |
| `update_plan_post` | Edit a post waiting for review, including its day and time |
| `rewrite_plan_post` | Rewrite a planned post's caption or image idea |
| `make_plan_post_image` | Make a planned post's image |
| `approve_plan_posts` | Approve one post or the whole plan |
| `remove_plan_post` | Remove a post waiting for review |
| `discard_plan` | Discard a plan |

Add an image by link, or use a file already in your Sociro media library.

## Safety

- Nothing is published by accident: a post needs a time, or a clear instruction to publish now. A draft is never sent by itself.
- No double posts: if the assistant repeats a request, the post that already exists is returned.
- Your assistant asks before it posts, edits or cancels, when your app is set to ask for approval.
- A connection can only do what you allowed when you signed in, or what its API key can do, and your plan's limits apply.
- A sign-in is short-lived and renews itself. Only the workspace owner or an admin can approve one, and it stops if they leave the workspace.
- The server stores no key and nothing about you between requests.
- A link to a private or local address is refused when adding an image.
- Disconnect an assistant at any time in Sociro under **AI apps**.

## What it does not do

It does not read or answer comments, show analytics beyond Facebook, Instagram and Threads, add a video from a link,
or connect social accounts (that is done in the Sociro app). A post's platform options (a video title, privacy, the
first comment) are set when it is created; to change them, cancel the post and create it again.

## Example prompts

- "List my Sociro accounts."
- "Schedule this for tomorrow at 9am on my café brand: Autumn menu starts Monday. Come in and try the maple latte."
- "Give X a shorter caption and add this image: https://example.com/latte.jpg"
- "What is scheduled for this week?"
- "Plan a week of posts for our autumn menu launch, then show me the drafts."
- "Move Thursday's post to 6pm, then approve the whole plan."

## Help

- Guides and troubleshooting: [sociro.com/mcp](https://sociro.com/mcp)
- Developer API: [app.sociro.com/docs](https://app.sociro.com/docs)
- Support: [sociro.com/contact](https://sociro.com/contact)

## Licence

The documentation in this repository is under the MIT licence. Sociro and the hosted server are the property of their owners.
