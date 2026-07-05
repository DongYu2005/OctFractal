# OctFractal: 三维分形生成 — 由粗到细的八叉树形状生成

北京大学《几何计算前沿》课程期末项目的总索引仓库。项目研究的核心问题是：

> 用由粗到细、**少步前向**的分形式生成器做三维形状生成，能否以远低于迭代自回归模型（OctGPT 约 576 次前向、单样本约 81 秒）的推理成本获得可用的形状质量？

项目在同一"由粗到细八叉树"框架下沿两条技术路线并行探索，分别在两个仓库中实现。本仓库以 git submodule 形式索引两者的最终版本。

## 仓库索引

| 子目录 | 仓库 | 路线 | 说明 |
|---|---|---|---|
| [`pku-3dfractal/`](https://github.com/DongYu2005/PKU-3Dfractalgeneration) | `DongYu2005/PKU-3Dfractalgeneration` | **路线 B：VQ token + 冻结 VQ-VAE 解码** | 生成器预测八叉树结构与叶子层 VQ token，复用 OctGPT 完全相同的冻结 VQ-VAE 解码为连续 SDF，质量差异可干净归因到生成器本身；生成器约 4 次前向对比 OctGPT 约 576 次（生成器约 300 倍加速）。环境配置、数据准备、训练/生成/评测命令与 OctGPT baseline 复现见该仓库 README |
| [`octfractal/`](https://github.com/2300094810/Octfractal) | `2300094810/Octfractal` | **路线 A：occupancy 输出的分形自回归生成** | 生成器直接输出叶子层离散占据体素，兄弟节点用 GRU 自回归建模，后处理近似转为 SDF/mesh，不依赖 VQ-VAE |

两条路线共享数据（OctGPT 预处理的 ShapeNet）与由粗到细生成骨架，差异集中在叶子输出表示与兄弟节点建模方式。

## 获取代码

```bash
# 连同两个子仓库一起克隆
git clone --recurse-submodules https://github.com/DongYu2005/OctFractal.git

# 已克隆的话补拉子模块
git submodule update --init
```

复现指引：路线 B 见 [`pku-3dfractal/README.md`](https://github.com/DongYu2005/PKU-3Dfractalgeneration#readme)（含权重下载、ShapeNet 预处理、训练/生成命令），路线 A 见 [`octfractal/README.md`](https://github.com/2300094810/Octfractal#readme)。

## 参考

- [OctGPT: Octree-based Multiscale Autoregressive Models for 3D Shape Generation](https://github.com/octree-nn/octgpt)（SIGGRAPH 2025）：本项目的 baseline 与冻结 VQ-VAE 来源，预训练权重见 HF [`wst2001/OctGPT`](https://huggingface.co/wst2001/OctGPT)
