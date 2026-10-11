# Agent / contributor notes (RDNA fork)

This repository is the **opengfx1030** vLLM fork for AMD **RDNA2 (gfx1030)** and related RDNA GPUs. Default branch: `rdna_extras`. Flash-Next / 0.31 bring-up also uses `rdna_extra/v0.31.0`.

## Do this first

1. Read the community wiki: https://blivioniag.github.io/gfx1030-wiki/
2. Prefer a **host venv** over Hub Docker for Flash-Next / HEAD kernels (published tags lag).
3. Serve with the in-tree launcher — do **not** invent a fresh `vllm serve` argv from memory:

```bash
MODEL=/path/to/checkpoint VENV=/path/to/venv \
  bash tools/rdna/serve_rdna.sh RECIPE=<name>
```

Dry-run: `PRINT=1 bash tools/rdna/serve_rdna.sh RECIPE=<name>`.

Recipes live in `tools/rdna/recipes/*.env` (Flash-Next: `flashnext`, `flashnext-mtp0`, `flashnext-mtp2`; dense: `27b-awq`, `27b-exl3`; DeepSeek-V4: `dsv4-flash`).

## Common mistakes

- Building Hub / Dockerfile images with `VLLM_REF=main` expecting Flash-Next — use a host checkout + launcher.
- Pasting old env dumps that set `VLLM_USE_BREAKABLE_CUDAGRAPH=1` on Flash-Next (drop it unless the recipe sets it, e.g. `dsv4-flash`).
- Forcing `VLLM_RDNA_FUSED_HC=0` on current Flash-Next recipes.
- Assuming `VLLM_PLE_QUANT_DIR` is required on every branch — check the recipe header for that branch (v0.31 uses upstream Engram PLE; fork INT4 sidecar may be reintroduced).

## Where docs live

| Topic | Location |
|---|---|
| Human wiki (setup, recipes, troubleshooting) | https://blivioniag.github.io/gfx1030-wiki/ |
| Serve launcher + recipes | `tools/rdna/serve_rdna.sh`, `tools/rdna/recipes/` |
| Kernel / V620 notes | `docs/rdna2/` |
| TunableOp profiles | `tunableop/` |

Do not invent tok/s or hardware claims. Prefer updating a recipe file over hard-coding host paths.
