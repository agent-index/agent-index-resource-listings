# marketplace

The public directory index for the agent-index ecosystem. Contains the registries that `create-org`, `check-updates`, and the marketplace collection use to discover and download collections and filesystem adapters.

## Contents

| File | Purpose |
|---|---|
| `marketplace-directory.json` | Registry of available marketplace collections (projects, strategy, capture, etc.) |
| `filesystem-adapter-directory.json` | Registry of available filesystem adapters (gdrive, onedrive, s3) |

## How It's Used

**Collections:** The `agent-index-marketplace` collection reads `marketplace-directory.json` to list available collections, check for updates, and download new collections via their `zip_url`.

**Filesystem Adapters:** `create-org` reads `filesystem-adapter-directory.json` to present available storage backends during org setup. It downloads the chosen adapter's pre-built bundle from the adapter repo's `zip_url` and includes it in the bootstrap zip.

**Caching:** Orgs cache the marketplace directory on the remote filesystem at `/shared/marketplace-cache/` for offline access. The cache is refreshed based on `marketplace_cache_ttl_hours` in `agent-index.json`.

## Adding a New Entry

**Collections:** Submit a PR adding an entry to `marketplace-directory.json` with the collection's name, description, version, repo URL, and zip URL. The collection repo must conform to the collection spec in `agent-index-core`.

**Filesystem Adapters:** Submit a PR adding an entry to `filesystem-adapter-directory.json` with the adapter's backend ID, display name, version, repo URL, and zip URL. The adapter repo must conform to the `filesystem-adapter-spec.md` in `agent-index-meta-docs`.

## License

Proprietary — Copyright (c) 2026 Agent Index Inc. All rights reserved. See [LICENSE](LICENSE) for details.
