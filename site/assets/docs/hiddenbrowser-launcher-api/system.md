# System

## Liveness probe

> Returns basic server info. The only endpoint that does \*\*not\*\* require auth.

```json
{"openapi":"3.0.3","info":{"title":"HiddenBrowser Launcher — Automation API","version":"0.1.0"},"tags":[{"name":"System"}],"servers":[{"url":"http://127.0.0.1:40325","description":"Local launcher (default port; change in Settings)"}],"security":[],"paths":{"/health":{"get":{"tags":["System"],"summary":"Liveness probe","description":"Returns basic server info. The only endpoint that does **not** require auth.","responses":{"200":{"description":"Server is up.","content":{"application/json":{"schema":{"type":"object","properties":{"ok":{"type":"boolean"},"name":{"type":"string"},"version":{"type":"string"}}}}}}}}}}}
```
