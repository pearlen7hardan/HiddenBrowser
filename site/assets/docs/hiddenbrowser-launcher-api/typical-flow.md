# Typical flow

1. `GET /fingerprint/new/{platform}` → a fully uniquified fingerprint (random platform\_version, host-aware CPU/RAM, clamped screen, library noise prefs), **not** persisted.
2. Optionally edit the returned `fingerprint` object (e.g. flip `noise.fonts.enabled`, set `webrtc`, tweak `navigator`).
3. `POST /profiles` (or `POST /folders/{folder}/profiles`) with that object as `fingerprint` → the launcher stores it **verbatim** (create never re-randomizes).
4. `POST /profiles/{id}/start` → returns the CDP endpoint; connect with `browserWSEndpoint = cdp.web_socket_debugger_url`.
5. `POST /profiles/{id}/stop` when done.
