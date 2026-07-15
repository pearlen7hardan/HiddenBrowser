# Fingerprint object

The `fingerprint` returned by `GET /fingerprint/new*` and accepted by the create endpoints is the launcher's native FingerprintConfig. It is round-tripped verbatim, so the schema below documents the common sub-objects but is intentionally open (`additionalProperties: true`).
