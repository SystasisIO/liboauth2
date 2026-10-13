# Resource Verification Controls

This maintained branch starts from OpenIDC liboauth2 v2.4.0, commit
`85da4346435aec77f2ff11b529f2c1e8e5033c44`. It adds public controls needed by a
bounded resource-server adapter. HTTP remains libcurl-based; JOSE remains
CJOSE-based. The upstream license and attribution are unchanged.

## Supported Integration Profile

Consumers must explicitly disable verification-result caching with
`verify.cache=none`, keep TLS peer/hostname verification enabled, disable HTTP
redirects, set positive finite network timeouts, and bound response sizes.
Endpoint URLs and CA configuration come from trusted operator configuration,
never from token claims. Resource-server qualification does not cover the
unrestricted legacy/default configuration or optional web-server adapters.

The public `_ex` functions return per-call typed failure evidence. A completed
HTTP exchange and a successful credential decision are different results:
HTTP 503 can be a completed HTTP call but must never be an authenticated token.
Failure classification must not parse vendor log strings. Production consumers
should use a silent vendor logger and expose only safe application diagnostics.

JWKS controls bound the total key array, throttle repeated refresh, and share
refresh state across configuration clones. Clone-owned HTTP contexts preserve
configured limits and CA bundles. Applications still own bounded execution,
overall deadlines, cancellation, admission closure and draining of work.

Legacy function signatures remain available. A zero HTTP timeout is rejected;
this branch intentionally does not promise unrestricted behavioral compatibility.
The fork does not itself supply every resource policy: allowed algorithm, access
token purpose, required claims, issuer/audience and freshness remain explicit
consumer policy over cryptographically verified evidence.

## Qualification And Updates

Run the upstream Check suite and the consuming Forge qualification matrix for
every pinned update. Qualification is for an exact source/dependency revision,
not a general claim about upstream or this branch. The current dependency
candidate uses Jansson 2.14.1 and CJOSE 0.6.2.8; dependency upgrades require
rechecking licenses, security advisories, negative cases and both Linux/macOS.

Keep transport/error-control changes reviewable separately from host policy.
Prefer upstreamable changes and periodically re-evaluate whether upstream
provides the same public controls. Never silently substitute a system liboauth2
without these extensions or fall back to permissive verification.
