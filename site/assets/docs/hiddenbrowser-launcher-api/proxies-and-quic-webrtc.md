# Proxies & QUIC/WebRTC

On **every** launch (UI or API), before the browser starts, the bound proxy is probed live for UDP-relay (SOCKS5 UDP\_ASSOCIATE). If UDP works, QUIC is enabled and WebRTC may use the proxied UDP relay; otherwise QUIC is disabled and WebRTC collapses to TCP-only so the real IP can't leak. Timezone/locale/geo `auto` fields are resolved live through the proxy at launch too. Passing `proxy` (string) on a persistent create adds it to the store and full-tests it up front; a temporary profile keeps it inline.
