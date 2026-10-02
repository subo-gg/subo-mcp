# Subo MCP server

Run your Discord community's forms, surveys, polls and quizzes by asking your AI app. Describe what you want to
ask your members, and your AI app builds the project in Subo, checks the script, and launches it.
You never open the dashboard.

[Subo](https://subo.gg/) runs conversational surveys, forms, quizzes and polls natively in Discord and on the
web. MCP (Model Context Protocol) is the open standard AI apps use to connect to outside tools.
This repository holds the listing for the hosted Subo MCP server. The server itself runs on
Subo's infrastructure, so there is nothing to install or host.

- **Endpoint:** `https://api.subo.ai/mcp` (streamable HTTP)
- **Auth:** `Authorization: Bearer sbo_live_…`, using a Subo API key
- **Setup guide:** https://subo.gg/blog/connect-discord-community-to-ai-agent/
- **Registry name:** `gg.subo/survey-bot` on the official MCP Registry

## What your AI app can do

43 operations, covering essentially everything the Subo API can do, in one community:

- create, edit, clone, validate, open, close and delete projects (surveys and polls)
- write and edit a project's script block by block
- read responses and AI analysis
- manage members' XP and accomplishments
- browse and clone templates
- manage webhooks
- read and change community settings

## What it deliberately cannot do

- create, rotate or delete API keys
- change who has access to your community
- connect a new verified-audience source, which needs a person to sign in

## Every operation is labeled

Each operation carries one of four marks your AI app can read: read-only, normal change,
**Outward** (it reaches real people, such as launching a project or granting XP a member will
see), or **Destructive** (it deletes something). A well-behaved AI app asks you before Outward
and Destructive calls. Subo also enforces its own rules regardless of what the agent does: a
project that is still open cannot be deleted through the API at all.

## Connect it

1. **Create a key.** In the Subo web app, open the Account page
   (https://app.subo.gg/app/account), go to **API Access**, and create a key. Under "This key
   represents", choose **Bot or agent**. Creating a key takes the **Admin** role in the community.
   You can give the key **Creator**-level access to narrow what the agent can do.
2. **Add the connection.** The confirmation panel shows the config with your key already in it.

   Claude Code (run in a terminal, not in a chat, then restart Claude Code):

   ```bash
   claude mcp add --transport http --scope user subo https://api.subo.ai/mcp --header "Authorization: Bearer sbo_live_…"
   ```

   Other AI apps: add a remote (HTTP) server with a custom header. The config usually looks
   like this, though the file location and the name of the outer key vary by app:

   ```json
   {
     "mcpServers": {
       "subo": {
         "type": "http",
         "url": "https://api.subo.ai/mcp",
         "headers": { "Authorization": "Bearer sbo_live_…" }
       }
     }
   }
   ```

3. **Check it.** In Claude Code, type `/mcp`: `subo` should show as connected, with 43 tools.

We have tested Subo from Claude Code and Grok Bot. Any AI app that can reach a remote MCP
server with a custom header should work the same way. The full walkthrough, including key
safety and troubleshooting, is at
https://subo.gg/blog/connect-discord-community-to-ai-agent/.

## Keep your key safe

One key equals one community, and it acts with the access its owner has right now. Keep it in
your AI app's configuration file and out of chats, documents and screenshots. If it leaks,
revoke it on the Account page.

## Plans

Every plan that can create an API key includes the MCP server, with nothing extra to buy or
enable. Requests share the Public API rate limit for your plan. See https://subo.gg/pricing/.

## More

- Announcement: https://subo.gg/blog/subo-mcp-server/
- API quickstart and recipes: https://subo.gg/api/
- For AI agents: https://subo.gg/llms.txt
- Support: https://subo.gg/support/
