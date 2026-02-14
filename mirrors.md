# Mirror Settings Reference

These mirror settings are used by [Comfy-Org/desktop](https://github.com/Comfy-Org/desktop) for users in regions with restricted access to default package sources (e.g. China). Documented here for reference when implementing update functionality.

## PyPI Mirrors

Used as `--index-url` for non-torch pip packages (ComfyUI requirements, custom node dependencies).

Desktop also adds unused mirrors as `--extra-index-url` fallbacks ([`PYPI_FALLBACK_INDEX_URLS`](https://github.com/Comfy-Org/desktop/blob/main/src/constants.ts#L194-L198), applied via [`getPypiFallbackIndexUrls()`](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L159-L162)).

- Alibaba Cloud: `https://mirrors.aliyun.com/pypi/simple/`
- Tencent Cloud: `https://mirrors.cloud.tencent.com/pypi/simple/`
- USTC: `https://pypi.mirrors.ustc.edu.cn/simple/`
- SJTU: `https://pypi.sjtu.edu.cn/simple/`
- Official: `https://pypi.org/simple/`

Used in [`installComfyUIRequirements()`](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L814-L833) via [`getPipInstallArgs()`](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L60-L92) which adds `--index-url` and `--extra-index-url` flags.

## Torch Mirrors

Used as `--index-url` specifically for torch, torchvision, and torchaudio. Kept separate from the PyPI mirror because torch packages are hosted on device-specific indexes.

- Aliyun (cu121): `https://mirrors.aliyun.com/pytorch-wheels/cu121/`

Applied in [`installPytorch()`](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L627-L656). Falls back to device-specific defaults via [`getDefaultTorchMirror()`](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L99-L109).

## Python Install Mirror

Used via the `UV_PYTHON_INSTALL_MIRROR` environment variable ([`uvEnv` getter](https://github.com/Comfy-Org/desktop/blob/main/src/virtualEnvironment.ts#L151)) to redirect uv's managed Python downloads away from GitHub. Not relevant for standalone environments where Python is pre-bundled.

- `https://python-standalone.org/mirror/astral-sh/python-build-standalone`

## Desktop Settings Keys

Defined in [`DEFAULT_SETTINGS`](https://github.com/Comfy-Org/desktop/blob/main/src/config/comfySettings.ts#L5-L16), stored in `comfy.settings.json`, and loaded at startup in [`createVirtualEnvironment()`](https://github.com/Comfy-Org/desktop/blob/main/src/main-process/comfyInstallation.ts#L67-L75):

- `Comfy-Desktop.UV.PythonInstallMirror`
- `Comfy-Desktop.UV.PypiInstallMirror`
- `Comfy-Desktop.UV.TorchInstallMirror`
