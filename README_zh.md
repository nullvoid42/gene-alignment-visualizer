# 基因序列比对可视化工具

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen?style=flat-square)](https://nullvoid42.github.io/gene-alignment-visualizer)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)](https://developer.mozilla.org/zh-CN/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)
[![无依赖](https://img.shields.io/badge/dependencies-none-success?style=flat-square)](package.json)
[![在线演示](https://img.shields.io/badge/demo-live-blue?style=flat-square)](https://nullvoid42.github.io/gene-alignment-visualizer)

一个基于浏览器的交互式序列比对可视化工具。通过动画和逐步演示，帮助学生、教育工作者和研究人员理解 **Needleman-Wunsch**（全局比对）和 **Smith-Waterman**（局部比对）算法的核心原理。

> **在线演示：** [https://nullvoid42.github.io/gene-alignment-visualizer](https://nullvoid42.github.io/gene-alignment-visualizer)

---

## 目录

- [功能特性](#功能特性)
- [算法介绍](#算法介绍)
- [打分方案](#打分方案)
- [快速开始](#快速开始)
- [使用方法](#使用方法)
- [界面预览](#界面预览)
- [技术栈](#技术栈)
- [贡献指南](#贡献指南)
- [许可协议](#许可协议)

---

## 功能特性

- **双算法支持** — 切换 Needleman-Wunsch（全局）和 Smith-Waterman（局部）比对
- **多序列类型** — 支持 DNA、RNA 和蛋白质序列
- **DP 矩阵可视化** — 完整展示动态规划打分矩阵，包含每个格子的分值和方向箭头
- **步进动画演示** — 可调节播放速度的格子填充动画
- **Traceback 路径高亮** — 在矩阵上直接标注最优比对路径
- **比对结果展示** — 显示最终比对序列，并标注匹配/错配/插入
- **零依赖** — 纯浏览器运行，无需构建工具、npm 或框架
- **深色主题 UI** — GitHub 风格的深色配色，适合长时间使用

---

## 算法介绍

### Needleman-Wunsch（全局比对）

由 Saul Needleman 和 Christian Wunsch 于 1970 年提出，通过填充 (m+1) × (n+1) 的 DP 矩阵找到两条序列的**最优全局比对**，递推公式为：

```
F(i, j) = max(
    F(i-1, j-1) + s(a_i, b_j),   // 匹配 / 错配
    F(i-1, j)   + gap,            // 缺失
    F(i,   j-1) + gap             // 插入
)
```

初始化：`F(i, 0) = i × gap`，`F(0, j) = j × gap`

适用于**长度相近**且**具有同源关系**的序列（如同源蛋白家族）。

---

### Smith-Waterman（局部比对）

由 Temple Smith 和 Michael Waterman 于 1981 年提出，通过修改递推公式（不允许负分）来找出**最高分的局部子序列比对**：

```
H(i, j) = max(
    0,
    H(i-1, j-1) + s(a_i, b_j),   // 匹配 / 错配
    H(i-1, j)   + gap,            // 缺失
    H(i,   j-1) + gap             // 插入
)
```

Traceback 从**最大分值格子**开始，在遇到第一个 `0` 时终止。适用于在较长、差异较大的序列中识别**保守区域**。

---

## 打分方案

| 事件 | 分值 |
|------|-----:|
| 匹配 |  +2  |
| 错配 |  −1  |
| Gap  |  −2  |

以上参数为常用的教育默认值。当前版本未实现仿射 Gap 惩罚模型（gap-open + gap-extend）。

---

## 快速开始

无需安装。克隆或下载仓库后，直接用浏览器打开 `index.html` 即可。

```bash
git clone https://github.com/nullvoid42/gene-alignment-visualizer.git
cd gene-alignment-visualizer
open index.html        # macOS
# 或
xdg-open index.html    # Linux
# 或直接将 index.html 拖入浏览器
```

或访问在线版本：[https://nullvoid42.github.io/gene-alignment-visualizer](https://nullvoid42.github.io/gene-alignment-visualizer)

---

## 使用方法

1. **选择算法** — 点击顶部的 **Needleman-Wunsch** 或 **Smith-Waterman** 标签
2. **输入序列** — 在 *Sequence A* 和 *Sequence B* 输入框中输入或粘贴序列
3. **选择序列类型** — 从下拉菜单中选择 DNA、RNA 或 Protein
4. **运行** — 点击 **Run** 按钮开始动画
5. **调节速度** — 使用速度滑块控制动画播放速度
6. **查看详情** — 鼠标悬停单个矩阵格子，查看分值和方向指针
7. **重置** — 点击 **Reset** 清除矩阵，重新开始

**示例输入：**

```
DNA:     ACGTACGT  /  ACGTTGCA
RNA:     AUGCUAUG  /  AUGCUAUC
Protein: HEAGAWGHEE  /  PAWHEAE
```

---

## 界面预览

> *截图将在首发版本后添加。*

| DP 矩阵（Needleman-Wunsch） | Traceback 路径 | 比对结果 |
|---|---|---|
| ![DP Matrix placeholder](docs/screenshots/dp-matrix.png) | ![Traceback placeholder](docs/screenshots/traceback.png) | ![Result placeholder](docs/screenshots/result.png) |

---

## 技术栈

| 层次 | 技术 |
|------|------|
| 标记 | HTML5 |
| 样式 | CSS3（自定义属性、CSS Grid、Flexbox） |
| 逻辑 | 原生 JavaScript（ES6+） |
| 渲染 | DOM 操作 |
| 托管 | GitHub Pages |

无需构建工具、打包器或包管理器。

---

## 贡献指南

欢迎贡献！如需提出修改：

1. Fork 本仓库
2. 创建功能分支：`git checkout -b feature/affine-gap`
3. 提交修改，附上清晰的说明
4. 提交 Pull Request，描述动机和实现方案

可选贡献方向：
- 仿射 Gap 惩罚模型
- 多序列比对（MSA）
- FASTA 文件导入
- 比对结果导出（SVG / PNG）
- 无障碍改进（键盘导航、屏幕阅读器支持）

对于重大修改，请先提交 Issue 讨论方案，再投入开发。

---

## 许可协议

本项目基于 [MIT License](LICENSE) 发布。

---

## 参考文献

1. Needleman, S. B., & Wunsch, C. D. (1970). A general method applicable to the search for similarities in the amino acid sequence of two proteins. *Journal of Molecular Biology*, 48(3), 443–453.
2. Smith, T. F., & Waterman, M. S. (1981). Identification of common molecular subsequences. *Journal of Molecular Biology*, 147(1), 195–197.
3. Durbin, R., Eddy, S. R., Krogh, A., & Mitchison, G. (1998). *Biological Sequence Analysis*. Cambridge University Press.
