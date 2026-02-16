# ComfyUI Portable Build Reference

Notes on how the official ComfyUI portable build (github.com/comfyanonymous/ComfyUI) handles packaging.

## Stripping

Minimal — only 3 files removed from `python_embeded/Lib/site-packages/torch/lib/`:

- `dnnl.lib`
- `libprotoc.lib`
- `libprotobuf.lib`

Nothing else is stripped. No `.dist-info`, no test directories, no `functorch`.

## Compression

Relies on aggressive 7-Zip LZMA2 settings for size reduction:

```
7z a -t7z -m0=lzma2 -mx=9 -mfb=128 -md=768m -ms=on -mf=BCJ2
```

## Relevant Workflows

- `stable-release.yml` — main release build
- `release-stable-all.yml` — builds multiple CUDA/Python combinations
- `windows_release_dependencies.yml` — pre-builds and caches wheel dependencies
