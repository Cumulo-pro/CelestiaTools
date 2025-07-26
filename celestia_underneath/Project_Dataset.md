# Celestia Underneath — Project Dataset

> **Path:** `data/projects.json`
>
> A machine‑readable catalogue of all the projects building **on, with or under** Celestia’s modular data‑availability layer.
> It is the single source of truth consumed by the public dashboard, the OpenAPI endpoint and community scripts.

---

## 1. File format

```jsonc
[
  {
    "project": "Citrea",          // Human‑readable name  🔹 required
    "type": "layer",              // High‑level category  🔹 required (enum)
    "subType": "zk-rollup",       // Extra granularity      optional
    "status": "testnet",          // active | testnet …   🔹 required
    "tags": ["bitcoin","zk"],     // Free‑form labels       optional

    "web": "https://citrea.xyz/", // Canonical URL         🔹 required (uri)
    "twitter": "citrea_xyz",      // Handle, no @          optional
    "whyCelestia": "…",           // Tweet/blog explaining Celestia use 🔹 required
    "description": "…",           // ≤ 280 chars summary   🔹 required

    "ecosystem": "bitcoin",       // Community / L1 hub    🔹 required (enum)
    "builtOn": "zk-custom",       // Underlying stack      🔹 required (enum)

    "img": "citrea.jpg",          // Relative asset path    optional
    "github": "https://…",        // Org or repo           optional
    "docs": "https://…",          // Dev docs              optional
    "celenium": "https://…",      // Blob explorer link    optional

    "articles": [                 // Extra reading         optional
      { "title": "…", "url": "…" }
    ]
  }
]
```

*Each object must follow the JSON Schema in `data/schema.json`.  
Empty strings are **forbidden**—omit the key instead.*

---

## 2. Required keys & their meaning

| Key | Type | Description |
|-----|------|-------------|
| `project` | `string` | Public name. |
| `type` | `string(enum)` | Macro‑category (see §3). |
| `status` | `string(enum)` | `active`, `testnet`, `mainnet`, `inactive`. |
| `web` | `uri` | Landing page. |
| `whyCelestia` | `uri` | First‑party source confirming Celestia usage. |
| `description` | `string` | One‑liner (≤ 280 chars). |
| `ecosystem` | `string(enum)` | Main community / L1. |
| `builtOn` | `string(enum)` | SDK / stack. |

Optional keys: `subType`, `tags[]`, `twitter`, `img`, `github`, `docs`, `celenium`, `articles[]`.

---

## 3. Enumerations

### 3.1 `type`

`ai`, `defi`, `gaming`, `nft`, `social`, `oracle`, `infra`, `layer`, `tools`

<details><summary>Mapping examples</summary>

| Raw label | `type` | `subType` |
|-----------|--------|-----------|
| Finance, DeFi | `defi` | `dex`, `lending` |
| LiquidStake | `defi` | `liquid-staking` |
| RaaS, Framework | `infra` | `raas`, `framework` |
| ZK, L2, Rollup | `layer` | `zk-rollup`, `l2` |
| … | … | … |
</details>

### 3.2 `ecosystem`

`ethereum`, `cosmos`, `bitcoin`, `sovereign`, `modular`, `multichain`, `solana`, `other-l1`

### 3.3 `builtOn`

`rollkit`, `initia`, `op-stack`, `arbitrum-orbit`, `cosmos-sdk`,
`polygon-cdk`, `zk-custom`, `bvm`, `solana-vm`, `other`

---

## 4. Contributing

1. **Fork** → edit `data/projects.json` (alphabetical order).
2. Run `npm run lint` to auto‑format.
3. Run `npm run validate` to pass JSON‑Schema checks.
4. Open a PR; CI will re‑validate.

**Minimum fields:** `project`, `type`, `status`, `web`, `whyCelestia`,
`description`, `ecosystem`, `builtOn`.

---

## 5. Versioning & Changelog

Each merge to `main` bumps the dataset version in `CHANGELOG.md`.  
Releases (`vYYYY.MM.DD`) include a zipped `data/` and SHA‑256 checksums.

---

## 6. License

Content (JSON, docs) is released under **CC0‑1.0**.  
Logos remain property of their respective teams.
