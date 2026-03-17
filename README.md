# SageAttention Stable ABI Wheels

This fork publishes wheels built with:

* Python stable ABI (`cp39-abi3`, compatible with Python 3.9+)
* PyTorch stable ABI (compatible with PyTorch 2.9+)

The latest wheels support GTX 16xx, RTX 20xx/30xx/40xx/50xx, A100, H100, and AGX Orin (sm75/80/86/87/89/90/120). There are also reports that SageAttention works with B200 (sm100) and DGX Spark (sm121), but those kernels are not bundled in these wheels and require building from source.

The default branch for this fork is `abi3_stable`.

## Indexes

* `https://appmana.github.io/forks-sageattention-stable-abi/cu128/`
* `https://appmana.github.io/forks-sageattention-stable-abi/cu130/`

Each index contains one package:

* `sageattention`

## Install With pip

CUDA 12.8:

```bash
pip install torch==2.9.0 --index-url https://download.pytorch.org/whl/cu128
pip install sageattention --index-url https://appmana.github.io/forks-sageattention-stable-abi/cu128 --no-deps
```

CUDA 13.0:

```bash
pip install torch==2.9.0 --index-url https://download.pytorch.org/whl/cu130
pip install sageattention --index-url https://appmana.github.io/forks-sageattention-stable-abi/cu130 --no-deps
```

On Windows, install `triton-windows` separately before importing `sageattention`.

## Install With uv

CUDA 12.8:

```bash
uv pip install --system torch==2.9.0 --index-url https://download.pytorch.org/whl/cu128
uv pip install --system sageattention --index-url https://appmana.github.io/forks-sageattention-stable-abi/cu128 --no-deps
```

CUDA 13.0:

```bash
uv pip install --system torch==2.9.0 --index-url https://download.pytorch.org/whl/cu130
uv pip install --system sageattention --index-url https://appmana.github.io/forks-sageattention-stable-abi/cu130 --no-deps
```

`pyproject.toml` example for `uv`:

```toml
[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[[tool.uv.index]]
name = "sageattention-cu128"
url = "https://appmana.github.io/forks-sageattention-stable-abi/cu128"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu128" }
sageattention = { index = "sageattention-cu128" }
```

For `cu130`, replace both URLs with the `cu130` indexes.

## Build

Install the matching PyTorch build, then build from source.

With `uv`:

```bash
uv pip install --system packaging setuptools wheel numpy ninja
uv pip install --system torch==2.9.0 --index-url https://download.pytorch.org/whl/cu128
TORCH_CUDA_ARCH_LIST="8.0 8.6 8.7 8.9 9.0 10.0 12.0" python setup.py bdist_wheel --verbose
```

Or for CUDA 13.0:

```bash
uv pip install --system torch==2.9.0 --index-url https://download.pytorch.org/whl/cu130
TORCH_CUDA_ARCH_LIST="8.0 8.6 8.7 8.9 9.0 10.0 12.0 12.1" python setup.py bdist_wheel --verbose
```

The built wheel will be written to `dist/`.

## Dev Notes

* The wheels are built using the [workflow](https://github.com/woct0rdho/SageAttention/blob/main/.github/workflows/build-sageattn.yml)
    * It is tricky to specify both torch (from `download.pytorch.org`) and pybind11 (not in that index) in an isolated build environment. The simplest approach here is [simpleindex](https://github.com/uranusjr/simpleindex).
* CUDA kernels for sm80/89/90 are bundled in the wheels, and also sm120 for CUDA >= 12.8
* For Turing GPUs (GTX 16xx, RTX 20xx), SageAttention 2 runs Triton kernels, which are the same as SageAttention 1. If you want to help improve the CUDA kernels for Turing, you may see https://github.com/Ph0rk0z/SageAttention2/tree/updates
* Volta GPUs (V100) are not supported because they do not have int8 tensor core
* The wheels do not use CXX11 ABI
* We cannot publish the wheels to PyPI, because PyPI does not support multiple PyTorch/CUDA variants for the same version of SageAttention. Some people are working on this, see https://astral.sh/blog/introducing-pyx and https://wheelnext.dev/proposals/pep817_wheel_variant_support/
