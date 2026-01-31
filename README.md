# Uplift Modeling for Marketing Campaign Optimization  
## 基于 Criteo 数据集的营销活动增量效果建模（用户增长方向）

[![License](https://img.shields.io/badge/License-CC0%201.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)

> **Goal**: Identify users who are *persuadable* by ads — i.e., they only click or convert **because** they saw the ad.  
> **目标**：精准识别“广告敏感人群”——即仅因看到广告才产生点击或转化行为的用户，实现高效预算分配。

This project demonstrates a complete **uplift modeling pipeline** using the public [Criteo Uplift Dataset](https://www.kaggle.com/datasets/arashnic/uplift-modeling) (CC0 License). The solution is designed for **user growth**, **marketing efficiency**, and **causal inference** roles in tech companies.

本项目基于公开的 Criteo Uplift 数据集，完整实现从数据探索、模型训练到策略评估的全流程，适用于互联网公司**用户增长**、**智能营销**和**因果推断**相关岗位。

---

## 📊 Dataset Overview | 数据集概览

- **Source**: [Kaggle: Uplift Modeling, Marketing Campaign Data](https://www.kaggle.com/datasets/arashnic/uplift-modeling)
- **License**: CC0 (Public Domain)
- **Size**: ~13M rows (not included in this repo due to size; download from Kaggle)
- **Key Columns**:
  - `treatment`: 1 = exposed to ad, 0 = control group （1=曝光组，0=对照组）
  - `exposure`: whether user actually saw the ad (only valid if `treatment=1`)
  - `visit`: binary label for click/visit （是否访问）
  - `conversion`: binary label for purchase/conversion （是否转化）
  - `f0`–`f11`: anonymized user features （匿名化用户特征）

> ⚠️ **Note**: The raw dataset is **not committed** to this repository due to its large size (~1.5GB). Please download it from Kaggle before running the code.

> ⚠️ **注意**：原始数据集较大，未包含在本仓库中。请先从 Kaggle 下载 `criteo-uplift-v2.1.csv` 并置于 `/kaggle/input/uplift-modeling/`（Kaggle）或 `./data/`（本地）。

---

## 🧠 Methodology | 方法论

We implement the **S-Learner** approach with **XGBoost Regressor**:

1. Treat `treatment` as an input feature.
2. Train a single model to predict outcome (`visit` or `conversion`) given features + treatment status.
3. Estimate individual uplift as:  
   `uplift = P(Y=1 | X, T=1) - P(Y=1 | X, T=0)`
4. Rank users by uplift score for targeted campaigns.

采用 **S-Learner + XGBoost 回归** 方案：
- 将 `treatment` 作为模型输入特征
- 用单一模型预测不同干预下的响应概率
- 个体 Uplift = 曝光预测值 - 未曝光预测值
- 按 Uplift 排序，指导精准投放

---

## 📈 Evaluation Metrics | 评估指标

| Metric | Description | Why It Matters |
|-------|-------------|----------------|
| **Qini Coefficient** | AUC of normalized incremental gain curve minus random baseline | Higher = better targeting efficiency |
| **Top-K Capture Rate** | % of total incremental visits captured in top K% of ranked users | Measures concentration of value |

- **Qini 系数**：衡量模型相比随机策略的增量收益优势
- **Top-K 捕获率**：评估高价值人群的集中度（如 Top 10% 覆盖多少总增量）

Example output from our model:

Qini Coefficient: 0.0187
Top 10% capture: 28.4%
Top 20% capture: 46.1%



> ✅ This means targeting the top 10% of users by uplift score captures **28.4% of all possible incremental visits** — far better than random (10%).

> ✅ 表明对 Uplift 排名前 10% 的用户投放，可捕获 **28.4% 的总增量访问**，远优于随机投放（仅 10%）。

---

## 🚀 How to Run | 运行方式

### On Kaggle (Recommended)
1. Fork this notebook on Kaggle
2. Attach the dataset: `arashnic/uplift-modeling`
3. Run all cells — no setup needed!

### Locally


Generated visualizations include:
- Feature correlation heatmap
- Uplift score distribution
- Gain curve & Qini curve

输出图表包括：
- 特征相关性热力图
- Uplift 分布直方图
- 增益曲线与 Qini 曲线

---

## 💼 Why This Matters for Hiring | 为何适合应聘展示

- ✅ **Business-aligned**: Solves real marketing ROI problem  
  **业务导向**：直击广告投放 ROI 优化痛点
- ✅ **Causal thinking**: Goes beyond correlation to estimate true incrementality  
  **因果思维**：超越相关性，量化真实增量
- ✅ **Production-ready**: Clean, modular, well-documented code  
  **工程规范**：代码清晰、可复现、易扩展
- ✅ **Metrics-driven**: Uses industry-standard uplift evaluation  
  **指标驱动**：采用业界标准评估体系

Perfect for roles in:
- User Growth / Growth Engineering
- Marketing Data Science
- Causal Inference / Experimentation

适用于以下岗位：
- 用户增长 / 增长工程师
- 营销数据科学家
- 因果推断 / 实验科学

---

## 📜 License

Code: MIT License  
Dataset: [CC0 1.0 Universal (Public Domain)](https://creativecommons.org/publicdomain/zero/1.0/)
