# Changelog

## 1.9.2

Hardening release for the Listen Together server and its CI. The app itself
changes only in its version number; nothing in the Android sources was touched.

### Listen Together server
- **Rate limits could be bypassed.** With `JAM_TRUST_PROXY=true` the client
  address was read from the *first* `X-Forwarded-For` entry, which the caller
  controls. It now reads the *last* entry, the one the proxy appended. The Caddy
  deploy already sent a single resolved address, so it is unaffected; a Render
  deploy was exposed. Assumes one trusted proxy hop (see `backend/README.md`).
- Fixed a data race: `autoplayEnabled` on a new party was written before the
  party lock was taken.
- The invite page for a malformed code showed `&amp;` (escaped twice) and echoed
  the whole path segment; it is now escaped once and capped at 32 characters.
- The invite page now sends `X-Content-Type-Options`, `X-Frame-Options: DENY`
  and `Referrer-Policy: no-referrer`.
- `gofmt` applied to the backend (whitespace only).

### CI
- `Check` fails on unformatted Go.

### Versions
- Phone, watch and TV: `1.9.2` (versionCode 40).
