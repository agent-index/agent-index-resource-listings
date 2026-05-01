# agent-index-resource-listings

The public directory index for the agent-index ecosystem. Contains the registries that `create-org`, `check-updates`, and the marketplace collection use to discover and download collections and filesystem adapters.

## Contents

| File | Purpose |
|---|---|
| `marketplace-directory.json` | Registry of available marketplace collections (projects, strategy, capture, etc.) |
| `filesystem-adapter-directory.json` | Registry of available filesystem adapters (gdrive, onedrive, s3) |
| `infrastructure-directory.json` | Registry of infrastructure pieces (agent-index-core, agent-index-marketplace) — required for every install, not optional |

## How It's Used

**Collections:** The `agent-index-marketplace` collection reads `marketplace-directory.json` to list available collections, check for updates, and download new collections via their `zip_url`.

**Filesystem Adapters:** `create-org` reads `filesystem-adapter-directory.json` to present available storage backends during org setup. It downloads the chosen adapter's pre-built bundle from the adapter repo's `zip_url` and includes it in the bootstrap zip.

**Infrastructure:** `check-updates` reads `infrastructure-directory.json` to determine the latest available versions of `agent-index-core` and `agent-index-marketplace`. Unlike marketplace collections, infrastructure pieces are not optional — every install has them — so they're listed separately to keep the marketplace directory's semantic clean (its consumers iterate "what can I install?" and shouldn't see infrastructure as installable). When a new core or marketplace release ships, this file MUST be updated; otherwise admins running `check-updates` won't see that an infrastructure update is available. The `private: true` flag (set on agent-index-core) signals that the public zip_url may not be reachable for fetching — admins manage core installs separately, but the version field is still the canonical "latest available" reference.

**Caching:** Orgs cache the marketplace directory on the remote filesystem at `/shared/marketplace-cache/` for offline access. The cache is refreshed based on `marketplace_cache_ttl_hours` in `agent-index.json`.

## Adding a New Entry

**Collections:** Submit a PR adding an entry to `marketplace-directory.json` with the collection's name, description, version, repo URL, and zip URL. The collection repo must conform to the collection spec in `agent-index-core`.

**Filesystem Adapters:** Submit a PR adding an entry to `filesystem-adapter-directory.json` with the adapter's backend ID, display name, version, repo URL, and zip URL. The adapter repo must conform to the `agent-index-filesystem` SPEC.

**Infrastructure releases:** When you ship a new version of `agent-index-core` or `agent-index-marketplace`, update the corresponding `current_version` in `infrastructure-directory.json` and bump `last_updated`. This is the SAME release where you bump the package's own `collection.json` `version` — the listing repo update is part of the release, not a follow-up. The preflight task in the developer collection checks for this consistency.

## License

Proprietary — Copyright (c) 2026 Agent Index Inc. All rights reserved. See [LICENSE](LICENSE) for details.
