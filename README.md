# OctFractal: 三维分形生成 — 由粗到细的八叉树形状生成

北京大学《几何计算前沿》课程期末项目的总索引仓库。项目研究的核心问题是：

> 一个**单次前向（约 4 次 forward）**的由粗到细八叉树生成器，能否逼近 OctGPT 这类迭代自回归模型（576 次前向、单样本约 43 秒）的形状质量？

项目沿两条技术路线并行探索，分别在两个仓库中实现。本仓库以 git submodule 形式索引两者的最终版本。

## 仓库索引

| 子目录 | 仓库 | 路线 | 说明 |
|---|---|---|---|
| [`pku-3dfractal/`](https://github.com/DongYu2005/PKU-3Dfractalgeneration/tree/feat/v5-flag-scaffolding) | `DongYu2005/PKU-3Dfractalgeneration`（分支 `feat/v5-flag-scaffolding`） | 基于 VQ-VAE 的 token 生成 | 复用 OctGPT 冻结的 VQ-VAE 作为叶子解码器，生成器只预测八叉树结构与叶子层 VQ token，质量差异可干净归因到生成器本身。含报告 `report/final_ydg.pdf`、实验配置与复现脚本（见 `SUBMISSION.md`） |
| [`octfractal/`](https://github.com/2300094810/Octfractal) | `2300094810/Octfractal` | 直接占据预测 | 生成器直接输出体素占据，经 `occupancy_to_sdf.py` 转为 SDF 并提取 mesh，不依赖 VQ-VAE |

## 获取代码

```bash
# 连同两个子仓库一起克隆
git clone --recurse-submodules https://github.com/DongYu2005/OctFractal.git

# 已克隆的话补拉子模块
git submodule update --init
```

各仓库的环境配置、数据预处理与训练/生成命令见各自的 README（`octfractal/README.md`）与提交清单（`pku-3dfractal/SUBMISSION.md`）。

## 参考

- OctGPT: Octree-based Multiscale Autoregressive Models for 3D Shape Generation（本项目的 baseline 与 VQ-VAE 来源，权重见 HF `wst2001/OctGPT`）
