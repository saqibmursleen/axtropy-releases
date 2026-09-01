# Apex releases

Update manifests and payloads for **Apex** by Axtropy.

This repository is public so that installed copies can check for updates without
carrying a credential. It holds no source: the payload is the released build,
and a build is inert without a valid subscription — access is enforced by the
sign-in gate, not by the payload being hard to obtain.

## What is here

| | |
|---|---|
| `latest.json` | the manifest Apex fetches: version, payload URL, SHA-256 |
| Releases | the payload zip for each version, attached as a release asset |

## How an update is applied

Apex checks this manifest and reports whether a newer version exists. It does
not install anything itself — a process cannot replace its own running
executable on Windows. The supervisor performs the swap, and only after it has:

1. verified the payload's SHA-256 against the manifest,
2. checked every path in the payload against an allowlist of managed files,
3. refused the payload **whole** if anything falls outside that list.

A payload that tries to write outside its lane is either broken or hostile, and
neither deserves a partial apply. Account settings, credentials and the engine
itself are never touched by an update.

Applying is additionally gated behind an explicit switch on each machine, so an
update is staged and reported rather than applied unless an operator has armed
it.
