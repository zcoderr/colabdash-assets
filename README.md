# colabdash-assets

ColabDash 在 Colab VM 上用到的公开二进制资源，例如预编译的 wheel。
文件都放在 Release 附件里。ColabDash 按 Release 地址加 SHA-256 引用这些文件，校验不符就拒绝使用，
退回现场编译。

Public binary assets (prebuilt wheels) used by ColabDash on Colab VMs. Every file is pinned by SHA-256 in ColabDash.

## Releases

### `lightwan-kernels-1`

LightWan（LightX2V 原生 NVFP4 + 稀疏注意力）需要的三个 CUDA 扩展，免得每台新 VM 都重新编译一遍。

| wheel | 源码 |
|---|---|
| `sageattention-*.whl` | [ModelTC/SageAttention@ed92299](https://github.com/ModelTC/SageAttention/tree/ed922996e7eada59612f40701d479cec8f2308dd) |
| `spas_sage_attn-*.whl` | [ModelTC/SpargeAttn@9c3886b](https://github.com/ModelTC/SpargeAttn/tree/9c3886beb1077927d18ddb3f4f63ddde5964a513) |
| `lightx2v_kernel-*.whl` | [ModelTC/LightX2V@540102c](https://github.com/ModelTC/LightX2V/tree/540102c31eae93ed4ada01647144de9d9045fde3) `lightx2v_kernel/`，CUTLASS [NVIDIA/cutlass@db1c288](https://github.com/NVIDIA/cutlass/tree/db1c288993354c88e551c40c19a8fb93a774a241) |

构建环境：Colab G4（NVIDIA RTX PRO 6000 Blackwell，sm_120），Python 3.13，Colab 自带的 PyTorch 2.11.0+cu130，
`TORCH_CUDA_ARCH_LIST=12.0`。只能用于完全相同的 Python 与 PyTorch 版本、sm_120 GPU。
构建方式见 ColabDash 的 `lightwan_runtime.build_wheels()`。SHA-256 列在 Release 说明里。

## 许可

这些 wheel 由上游开源代码编译而成，仍遵循上游许可，许可文本在 [`licenses/`](licenses/)：

- SageAttention、SpargeAttn、LightX2V：Apache License 2.0
- CUTLASS（编译进 `lightx2v_kernel`）：BSD 3-Clause，Copyright (c) 2017 - 2026 NVIDIA CORPORATION & AFFILIATES

本仓库没有修改上游源码。
