# Security

Who this is for: operators preparing llmost for LAN or broader exposure.

What you will finish with: a practical, easy checklist for local-first safety and secure external access.

## Security Model In One Minute

- Local-first by default: gateway binds to `127.0.0.1`.
- Non-loopback bind (`0.0.0.0` or external interface): protected endpoints require bearer token by default.
- Hybrid override available: private subnet access without bearer can be enabled explicitly.
- `/health` is unauthenticated for diagnostics.

## Auth and Network Policy

Protected endpoints include:
- `/api/local-models/*`
- `/api/providers`
- `/api/test-connection`
- `/api/chat`
- `/v1/chat/completions`
- `/v1/models`

Unauthenticated endpoint:
- `/health`

Trusted private/local CIDR logic (for hybrid override):
- loopback: `127.0.0.0/8`, `::1/128`
- RFC1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`
- link-local: `169.254.0.0/16`
- ULA IPv6: `fc00::/7`

Important:
- trust decisions use `RemoteAddr`
- forwarded headers are not trusted for auth policy

## Hybrid Private-Subnet Override

Setting:
- `security_allow_private_subnet_without_bearer` (default `false`)

When `false`:
- non-loopback protected endpoints require bearer for all callers

When `true`:
- private/local subnet callers may access protected endpoints without bearer
- public-source callers still require bearer

Use this only on trusted networks.

## Secrets and File Permissions

- daemon bearer is passed via env (`LLMOST_BEARER_TOKEN`), not argv
- sensitive files are written with restricted permissions (`0600`)
- config/log/runtime dirs are restricted (`0700`)
- diagnostic outputs redact bearer/API-key values

## CORS Policy

- wildcard CORS is disabled
- only known local UI/gateway origins are allowed
- unknown origins receive no permissive CORS headers

## HTTP Abuse Hardening

- server read/write/idle timeouts are enabled
- oversized request bodies are capped
- upstream response reads are bounded
- JSON error handling avoids leaking internals

## Filesystem Safety (Model Lifecycle)

- physical deletion is allowed only under configured `models_root`
- paths outside `models_root` are unregistered only (no destructive delete)
- dangerous paths are explicitly rejected

## Secure External Access Checklist

1. Keep default host `127.0.0.1` unless LAN access is required.
2. If binding non-loopback, set a strong bearer token.
3. Keep `security_allow_private_subnet_without_bearer=false` unless you intentionally trust the subnet.
4. Keep pprof disabled unless actively debugging.
5. Run:

```bash
./scripts/security-audit.sh
./bin/llmost doctor
./bin/llmost status
```

6. Verify gateway port ownership before sharing endpoint access.

## Ongoing Release Gate

- CI runs `scripts/security-audit.sh`
- release is blocked on HIGH/CRITICAL reachable findings
- advisories in vendored dependencies are tracked separately from llmost-owned patch scope
