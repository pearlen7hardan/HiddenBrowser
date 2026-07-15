# Proxies

## List stored proxies

> Proxies saved in the launcher store. \*\*Credentials (username/password) are never returned.\*\*

```json
{"openapi":"3.0.3","info":{"title":"HiddenBrowser Launcher — Automation API","version":"0.1.0"},"tags":[{"name":"Proxies"}],"servers":[{"url":"http://127.0.0.1:40325","description":"Local launcher (default port; change in Settings)"}],"security":[{"bearerAuth":[]}],"components":{"securitySchemes":{"bearerAuth":{"type":"http","scheme":"bearer","bearerFormat":"JWT","description":"Permanent JWT from Settings → Automation API."}},"schemas":{"ProxySummary":{"type":"object","description":"Stored proxy without credentials.","properties":{"id":{"type":"string"},"name":{"type":"string"},"kind":{"type":"string","enum":["socks5","http","https"]},"host":{"type":"string"},"port":{"type":"integer"},"country":{"type":"string","description":"Informational tag (e.g. \"US\")."}}}},"responses":{"Unauthorized":{"description":"Missing or invalid Bearer token."}}},"paths":{"/proxies":{"get":{"tags":["Proxies"],"summary":"List stored proxies","description":"Proxies saved in the launcher store. **Credentials (username/password) are never returned.**","responses":{"200":{"description":"Proxy summaries.","content":{"application/json":{"schema":{"type":"array","items":{"$ref":"#/components/schemas/ProxySummary"}}}}},"401":{"$ref":"#/components/responses/Unauthorized"}}}}}}
```

## Add a proxy

> Adds a proxy to the store (deduped by endpoint). Pass \`proxy\` as a\
> string, or explicit \`host\`/\`port\` (+ \`kind\`/\`username\`/\`password\`).<br>

```json
{"openapi":"3.0.3","info":{"title":"HiddenBrowser Launcher — Automation API","version":"0.1.0"},"tags":[{"name":"Proxies"}],"servers":[{"url":"http://127.0.0.1:40325","description":"Local launcher (default port; change in Settings)"}],"security":[{"bearerAuth":[]}],"components":{"securitySchemes":{"bearerAuth":{"type":"http","scheme":"bearer","bearerFormat":"JWT","description":"Permanent JWT from Settings → Automation API."}},"schemas":{"AddProxyRequest":{"type":"object","description":"A proxy string OR explicit fields (string wins).","properties":{"proxy":{"type":"string","description":"`scheme://user:pass@host:port` or `host:port:user:pass`."},"kind":{"type":"string","enum":["socks5","http","https"]},"host":{"type":"string"},"port":{"type":"integer"},"username":{"type":"string"},"password":{"type":"string"},"name":{"type":"string"},"country":{"type":"string"},"notes":{"type":"string"}}},"ProxySummary":{"type":"object","description":"Stored proxy without credentials.","properties":{"id":{"type":"string"},"name":{"type":"string"},"kind":{"type":"string","enum":["socks5","http","https"]},"host":{"type":"string"},"port":{"type":"integer"},"country":{"type":"string","description":"Informational tag (e.g. \"US\")."}}},"Error":{"type":"object","properties":{"error":{"type":"string"}},"required":["error"]}},"responses":{"Unauthorized":{"description":"Missing or invalid Bearer token."}}},"paths":{"/proxies":{"post":{"tags":["Proxies"],"summary":"Add a proxy","description":"Adds a proxy to the store (deduped by endpoint). Pass `proxy` as a\nstring, or explicit `host`/`port` (+ `kind`/`username`/`password`).\n","requestBody":{"required":true,"content":{"application/json":{"schema":{"$ref":"#/components/schemas/AddProxyRequest"}}}},"responses":{"200":{"description":"Stored proxy summary.","content":{"application/json":{"schema":{"$ref":"#/components/schemas/ProxySummary"}}}},"400":{"description":"Unparseable proxy / missing host+port.","content":{"application/json":{"schema":{"$ref":"#/components/schemas/Error"}}}},"401":{"$ref":"#/components/responses/Unauthorized"}}}}}}
```

## DELETE /proxies/{id}

> Delete a proxy

```json
{"openapi":"3.0.3","info":{"title":"HiddenBrowser Launcher — Automation API","version":"0.1.0"},"tags":[{"name":"Proxies"}],"servers":[{"url":"http://127.0.0.1:40325","description":"Local launcher (default port; change in Settings)"}],"security":[{"bearerAuth":[]}],"components":{"securitySchemes":{"bearerAuth":{"type":"http","scheme":"bearer","bearerFormat":"JWT","description":"Permanent JWT from Settings → Automation API."}},"responses":{"Unauthorized":{"description":"Missing or invalid Bearer token."}}},"paths":{"/proxies/{id}":{"delete":{"tags":["Proxies"],"summary":"Delete a proxy","parameters":[{"name":"id","in":"path","required":true,"schema":{"type":"string"}}],"responses":{"200":{"description":"Deleted.","content":{"application/json":{"schema":{"type":"object","properties":{"deleted":{"type":"boolean"},"id":{"type":"string"}}}}}},"401":{"$ref":"#/components/responses/Unauthorized"}}}}}}
```
