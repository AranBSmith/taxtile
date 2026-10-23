# taxtile plugin for Claude Code

Gives Claude the Taxtile Irish tax calculator as tools (income tax, USC, PRSI, CGT, credits, take-home pay) plus an `irish-tax` skill that tells Claude when to use them, what to ask, and what to caveat. It uses the same `taxtile-core` calculation as the website. It runs locally over stdio and sends nothing over the network.

The MCP server is a self-contained bundle committed at `server/taxtile-mcp.js` (needs Node 20+ on your PATH; no npm install).

## Install from GitHub

The repo must be public (or you need access to it). In Claude Code:

```
/plugin marketplace add AranBSmith/taxtile
/plugin install taxtile@taxtile-marketplace
```

Or from a shell:

```bash
claude plugin marketplace add AranBSmith/taxtile
claude plugin install taxtile@taxtile-marketplace
```

## Try it from a local path

Without installing anything, load the plugin for one session:

```bash
claude --plugin-dir ./plugin
```

Or register the repo as a local marketplace:

```bash
claude plugin marketplace add .   # from the repo root
claude plugin install taxtile@taxtile-marketplace
```

Run `/reload-plugins` after editing, then `/mcp` to check the server is connected.

If you already registered the server by hand (`claude mcp add taxtile ...`), remove it first (`claude mcp remove taxtile`) so the tools are not offered twice. The plugin's server is named `plugin:taxtile:taxtile` and its tools are `mcp__plugin_taxtile_taxtile__*`.

## Usage

Just ask:

- "What is my take-home pay on 65,000 in 2026?"
- "I'm married, jointly assessed, I earn 70,000 and my partner 30,000. What do we pay in 2025?"
- "Compare 80,000 with and without a 10% pension contribution."
- "What are the USC bands for 2026?" / "Which tax credits can I claim?"
- "I sold shares for a 20,000 gain in 2026. What is the CGT?"

Claude asks for the tax year and marital status if you leave them out, and reports provisional years and what is not modelled.

## Updating the bundle

The bundle is built from the Taxtile source (taxtile-core and taxtile-mcp) and copied here as `plugin/server/taxtile-mcp.js`. Bump `version` in `plugin/.claude-plugin/plugin.json` when results change.

## Disclaimer

This is not financial advice. 2027 figures are provisional (Budget coverage) and 2018-2021 are unverified. Some known calculation issues are listed in each result's `notModelled`. Check anything that matters with Revenue or a tax adviser.
