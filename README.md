# dsh-greatarrow — GreatArrow.ai memory for DeepSeek Harness

Connects [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)
(`dsh`) to your [GreatArrow.ai](https://www.greatarrow.ai) workspace through
the GreatArrow.ai MCP server. Your agent can then search and save memories,
read and update tasks, and resume earlier sessions. The same memory is shared
with every other AI client you connect to GreatArrow.ai.

This is a configuration-only bundle. It adds one `@deepseek-ai/dsh-mcp-client`
row (Streamable HTTP, `https://www.greatarrow.ai/api/mcp`) to your dsh
composition. It ships no executable code and needs no build step.

## Install

1. Get a token: sign in at
   [greatarrow.ai/install/deepseek-harness](https://www.greatarrow.ai/install/deepseek-harness).
2. Put it in `~/.dsh/.env` (or `$DSH_HOME/.env`), which dsh loads at launch:

   ```sh
   GREAT_ARROW_TOKEN=gad_your_token
   ```

   Keep that file private (`chmod 600 ~/.dsh/.env`). The token is never
   written into the YAML.

3. Install the plugin into the profile you run (`dsh web` boots `web`):

   ```sh
   dsh plugin --profile web add github:GreatArrow-ai/dsh-greatarrow
   ```

   If you use the [dshmarket](https://github.com/dsh-market/dsh-market)
   plugin and this entry is listed there, **Settings → Plugin Market** installs
   it with one click.

4. Restart dsh. The tools appear as `mcp__great-arrow__<tool>`, for example
   `mcp__great-arrow__memory_search`, `mcp__great-arrow__memory_create`,
   `mcp__great-arrow__todo_list`. The server's instructions join the system
   prompt, so the agent knows when to search and when to save.

## Check it works

```sh
dsh --profile web --dump-config | grep -A8 great-arrow-memory
```

Then, in a new session, ask: _"What do you know about my workspace?"_ The
agent should call `mcp__great-arrow__memory_search`.

## Already ran the GreatArrow.ai installer?

The installer (Arrow Setup) writes the same row into `~/.dsh/cordis.patch.yml`,
which covers every profile. Use the installer **or** this plugin, not both. Two
rows with the same server name make the second one fail to load. When the
installer sees this plugin in a profile, it writes only the token.

## Options

| Environment variable | Default                     | Purpose                                      |
| -------------------- | --------------------------- | -------------------------------------------- |
| `GREAT_ARROW_TOKEN`  | — (required)                | Your `gad_` token                            |
| `GREAT_ARROW_URL`    | `https://www.greatarrow.ai` | Point at a non-production GreatArrow.ai host |

## Uninstall

```sh
dsh plugin --profile web remove dsh-greatarrow
```

Then delete the `GREAT_ARROW_TOKEN` line from `~/.dsh/.env`, and revoke the
token at [greatarrow.ai/account/connections](https://www.greatarrow.ai/account/connections).

## Privacy and support

Auth, scopes, rate limits and audit logging are enforced by the GreatArrow.ai
server. [Privacy](https://www.greatarrow.ai/legal/privacy) ·
[Support](https://www.greatarrow.ai/support) · `support@greatarrow.ai`

The source of truth for this repository lives in the GreatArrow.ai app repo
(`dsh-plugin/`). A test there keeps this patch identical to the one the
installer writes.
