# SageAttention Stable ABI Wheels

This fork publishes `cp39-abi3` wheels for PyTorch 2.9+.

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
