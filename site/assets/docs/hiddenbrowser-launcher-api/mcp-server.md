# MCP server

A companion **MCP** (Model Context Protocol) server lets an AI client drive the launcher through *this* API plus a profile's browser over CDP. It is a separate Node process — **not** part of this HTTP API and not hosted by it. Get it from the launcher (**Settings → MCP server → Download MCP server**, or the sidebar "Download MCP" button), which saves the source to a folder you choose; or from the repo at `rust/hiddenbrowser-launcher/mcp/`.

Under the hood it just calls this API: configure it with `HIDDENBROWSER_API` (this base URL) and `HIDDENBROWSER_TOKEN` (the Bearer token from *Settings → Automation API*). It exposes:

* **API tools** — thin wrappers over the endpoints below (profiles, fingerprints, proxies, folders, cookies, start/stop).
* **Browser tools** (patchright over CDP) — `browser_navigate`, `browser_evaluate`, `browser_screenshot`, `browser_click`, `browser_fill`, tabs, waits, etc., targeting a profile started via `POST /profiles/{id}/start` (which returns the CDP endpoint).

Run it standalone over **stdio** (the client spawns it) or, with `MCP_HTTP_PORT` set, over HTTP at `127.0.0.1:<port>/mcp`. See the MCP server's README for setup. (The launcher only downloads it; it does not run it.)
