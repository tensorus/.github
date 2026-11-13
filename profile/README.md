<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/tensorus/.github/main/assets/hero-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/tensorus/.github/main/assets/hero-light.svg">
  <img alt="Tensorus — The Agentic Tensor Database and Data Lakehouse" src="https://raw.githubusercontent.com/tensorus/.github/main/assets/hero-dark.svg" width="880">
</picture>

<br/>

**[tensorus.com](https://tensorus.com)** · **[Contact (`ai@tensorus.com`)](mailto:ai@tensorus.com)**

</div>

---

## Why Tensorus

Modern scientific computing, physical simulation, multimodal AI, and quantitative engineering run on **multidimensional numerical arrays**—not flat rows or isolated 1D embedding vectors. Yet teams still stitch together ad-hoc object buckets, unvalidated `.npy`/`.zarr` files, brittle scripts, and unconstrained AI agents that can silently corrupt units, masks, or provenance.

**Tensorus** is an **Agentic Tensor Database and Data Lakehouse** engineered from the ground up so developers, scientists, and user-delegated AI agents operate on one verifiable numerical foundation:

| Pillar | What Tensorus Guarantees |
| :--- | :--- |
| **Verifiable Numerical Identity** | Every tensor version is stored in immutable, content-addressed **Zarr v3** chunks with canonical little-endian **SHA-256** digests, lossless binary **NPY** streaming, optimistic head concurrency (`expected_version`), and idempotent retries. |
| **Declared Physical & ML Semantics** | First-class dimensional axes, physical units, coordinate systems (`v1`/`v2`), explicit boolean validity/missingness masks, and immutable dataset/release contracts prevent silent unit or axis mismatches before execution. |
| **Safe Delegated Agent Execution** | External AI agents (**Claude, Cursor, custom MCP hosts, and scoped SDK clients**) inspect schemas, run nondurable preflight (`explain`), and propose typed multi-step plans (`propose`). Execution requires **hash-bound human or trusted-application approval (`plan_hash`)**, runs on lease-fenced workers across **65 bounded numerical kernels**, and emits auditable cryptographic receipts. |
| **Enterprise Governance & Cloud Durability** | Workspace isolation, OIDC/PKCE identity & multi-workspace organizations, **transitive lineage Dataset ACLs** (`restricted_acl_v1`), outbound egress allowlists, two-phase physical garbage collection, paired catalog/object recovery with incarnation writer fences, and regional cell authority. |

---

## Unified Platform Architecture

<div align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/tensorus/.github/main/assets/architecture-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/tensorus/.github/main/assets/architecture-light.svg">
  <img alt="Tensorus Unified Platform Architecture" src="https://raw.githubusercontent.com/tensorus/.github/main/assets/architecture-dark.svg" width="880">
</picture>
</div>

---

## Unified SDK & Built-In MCP Integration

Tensorus ships a single, unified client distribution (`tensorus`) that includes **both** the Python HTTP SDK (`tensorus.Client`), **Xarray** labeled-array interoperability (`tensorus[xarray]`), and the **Model Context Protocol (MCP)** stdio bridge (`tensorus[mcp]` / `tensorus-mcp`)—eliminating separate, out-of-sync agent adapters:

```python
import numpy as np
from tensorus import Client

with Client("https://api.tensorus.com", api_key="tns_...") as client:
    # 1. Create a governed dataset and store a canonical N-dimensional array
    client.create_dataset("ocean_argo", description="Global Argo float temperature grids")
    temperature = np.asarray([[289.15, 290.40], [288.90, 291.05]], dtype="float32")

    v1 = client.put(
        "ocean_argo",
        "sea_surface_temp",
        temperature,
        expected_version=0,
        metadata={"source": "argo-buoy-array", "unit": "K"},
    )

    # 2. Propose a bounded numerical workflow (agents propose; humans/apps approve exact plan_hash)
    plan = client.propose_plan(
        dataset="ocean_argo",
        title="Normalize sea surface temperature",
        steps=[{"id": "norm", "operation": "normalize", "inputs": [v1["id"]], "output": "sst_norm"}],
    )
    client.approve_plan(plan["id"], plan["plan_hash"])
```

---

<div align="center">

**Built for global scientific, ML, and agentic data engineering teams.**  
[tensorus.com](https://tensorus.com) · `ai@tensorus.com`

</div>
