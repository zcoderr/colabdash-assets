# colabdash-assets

Public binaries that ColabDash uses on Colab VMs, such as prebuilt wheels and executables. The files are attached to
the releases. ColabDash pins each one by its release URL and SHA-256; a file that does not match is refused, and
ColabDash builds from source on the VM instead.

## Releases

### `lightwan-kernels-1`

The three CUDA extensions that LightWan (LightX2V's native NVFP4 path with sparse attention) needs, so that a new VM
does not compile them again.

| Wheel | Source |
|---|---|
| `sageattention-*.whl` | [ModelTC/SageAttention@ed92299](https://github.com/ModelTC/SageAttention/tree/ed922996e7eada59612f40701d479cec8f2308dd) |
| `spas_sage_attn-*.whl` | [ModelTC/SpargeAttn@9c3886b](https://github.com/ModelTC/SpargeAttn/tree/9c3886beb1077927d18ddb3f4f63ddde5964a513) |
| `lightx2v_kernel-*.whl` | [ModelTC/LightX2V@540102c](https://github.com/ModelTC/LightX2V/tree/540102c31eae93ed4ada01647144de9d9045fde3) `lightx2v_kernel/`, with CUTLASS [NVIDIA/cutlass@db1c288](https://github.com/NVIDIA/cutlass/tree/db1c288993354c88e551c40c19a8fb93a774a241) |

Built on a Colab G4 (NVIDIA RTX PRO 6000 Blackwell, sm_120) with Python 3.13, Colab's PyTorch 2.11.0+cu130 and
`TORCH_CUDA_ARCH_LIST=12.0`. They work only with exactly that Python and PyTorch, on sm_120 GPUs. They are built by
ColabDash's `lightwan_runtime.build_wheels()`. The SHA-256 of each file is in the release notes.

### `audiocpp-drama3-1`

`audiocpp_server` from audio.cpp, for serving Fish Audio Drama 3 on a Colab L4, so that a new VM does not spend about
ten minutes building it.

| Archive | Source |
|---|---|
| `audiocpp-cc21c9ea0231-fish_drama3-sm89-linux-x64.tar.gz` | [0xShug0/audio.cpp@cc21c9e](https://github.com/0xShug0/audio.cpp/tree/cc21c9ea02316c600a2a77641f133e3c3252a5b7) |

Built on a Colab L4 (sm_89, driver 580.82.07, Ubuntu 24.04, nvcc 13.0.88) with audio.cpp's
`scripts/build_linux.sh --backend cuda --build-type Release --cuda-arch 89 --model-set custom --models fish_drama3
--target audiocpp_server`. It runs only on sm_89 GPUs and only serves the fish_drama3 model family. It uses the CUDA
libraries of the Colab image (`libcudart.so.13`, `libcublas.so.13`, `libcublasLt.so.13`, `libnccl.so.2`), which are not
bundled. It is built by ColabDash's `studio/tts_runner.build_binary()`.

The archive holds `bin/audiocpp_server`, audio.cpp's `LICENSE`, its `model_specs/`, and `BUILD.json` (commit, CUDA
architecture and `ldd` output). It holds no model weights; ColabDash downloads those from Hugging Face. The SHA-256 is
in the release notes.

## Licenses

These files are compiled from upstream open-source code and remain under the upstream licenses. The license texts are
in [`licenses/`](licenses/).

- SageAttention, SpargeAttn, LightX2V: Apache License 2.0.
- CUTLASS (compiled into `lightx2v_kernel`): BSD 3-Clause, Copyright (c) 2017 - 2026 NVIDIA CORPORATION & AFFILIATES.
- audio.cpp: Apache License 2.0, Copyright 2026 ShugoAI LLC. `audiocpp_server` also contains third-party code that
  audio.cpp keeps under `external/`:
  - ggml: MIT, Copyright (c) 2023-2026 The ggml authors.
  - SentencePiece: Apache License 2.0, with its bundled Abseil (Apache License 2.0), darts-clone (BSD 3-Clause),
    esaxx (MIT) and protobuf-lite (BSD 3-Clause).
  - cJSON, cpp-httplib, libyaml: MIT.
  - utf8proc: MIT ("expat"), with the Unicode data license for its data tables.
  - The tokenizer files in `external/llama_tokenizer` come without a license file of their own; they match the
    unicode tokenizer files of llama.cpp (MIT, The ggml authors, the same notice as `ggml-LICENSE.txt`).

No upstream source was modified.
