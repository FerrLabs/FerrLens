<div align="center">

<h1><img src=".github/logo.svg" alt="" width="48" height="48" align="absmiddle"> FerrLens</h1>

**Run DNS, email, HTTP and SEO checks from an AI assistant.**

MCP server for [FerrLens](https://ferrlabs.com), a set of network and website diagnostics.

[![npm @ferrlens/mcp](https://img.shields.io/npm/v/@ferrlens/mcp)](https://www.npmjs.com/package/@ferrlens/mcp)
[![CI](https://github.com/FerrLabs/FerrLens/actions/workflows/ci.yml/badge.svg)](https://github.com/FerrLabs/FerrLens/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/FerrLabs/FerrLens)](LICENSE)

[MCP server](mcp/README.md) | [npm](https://www.npmjs.com/package/@ferrlens/mcp) | [FerrLabs](https://ferrlabs.com)

</div>

## What is in this repository

This repository holds the open source parts of FerrLens. For now that is the MCP server in [`mcp/`](mcp), published to npm as [`@ferrlens/mcp`](https://www.npmjs.com/package/@ferrlens/mcp). The product itself lives in [FerrLens-Cloud](https://github.com/FerrLabs/FerrLens-Cloud).

## MCP server

All tools are read-only and anonymous. They cover DNS lookups and propagation, SPF/DMARC/DKIM, blocklists, address verification, HTTP and security headers, page metadata and Open Graph previews, mixed content, CORS, certificate transparency search, SEO (Lighthouse), robots.txt, sitemaps and link checks. Targets must be reachable from the internet; the API refuses private addresses.

### Install

Add it to your MCP client configuration (Claude Code, Cursor, etc.):

```json
{
  "mcpServers": {
    "ferrlens": { "command": "npx", "args": ["-y", "@ferrlens/mcp"] }
  }
}
```

No account, no token and no OAuth flow. The API rate limits each caller by IP.

### Configuration

| Variable | Description | Default |
| --- | --- | --- |
| `FERRLENS_API_URL` | Base URL of the FerrLens API. | `https://api.ferrlens.com` |
| `FERRLENS_API_VERSION` | Contract version sent as `x-ferrlens-api-version`. | `2026-08-04` |

The full tool list, resources, prompts and the shared variables (token, OAuth, host allowlist, HTTP transport, output cap) are documented in [`mcp/README.md`](mcp/README.md) and the [FerrLabs MCP README](https://github.com/FerrLabs/FerrLabs/blob/main/packages/mcp/README.md#configuration).

## Development

Node 22 or later and pnpm 12.

```bash
cd mcp
pnpm install
pnpm build
pnpm test
pnpm smoke
```

`pnpm smoke` boots the server over stdio and checks the handshake and the advertised tools.

## License

Mozilla Public License 2.0, see [LICENSE](LICENSE).
