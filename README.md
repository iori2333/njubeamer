# njubeamer

南京大学（NJU）风格 LaTeX Beamer 演示文稿模板。

## 特点

- **单文件主题**：只需 `njubeamer.sty` 和一个 `assets/logos/` 文件夹即可使用
- **四种页面样式**：标题页、目录页、章节标题页、正文页
- **南大紫配色**：主色调采用南京大学标志性紫色
- **自动章节页**：每个 `\section` 自动插入章节标题页
- **中文支持**：基于 `ctex` 宏包，开箱即用

## 文件结构

```
.
├── njubeamer.sty          # 主题文件（颜色 + 外框 + 内部样式）
├── njubeamer-demo.tex     # 示例文档
├── .latexmkrc             # latexmk 编译配置
├── README.md
└── assets/
    └── logos/
        ├── nju-emblem.pdf
        ├── nju-emblem-purple.pdf
        ├── nju-name.pdf
        └── nju-name-purple.pdf
```

> **免责声明**：本项目 100\% 由 Agent Coding（纯添加、无自然）完成。如果你在编译时遇到问题，建议先咨询你自己的 AI Buddy —— 毕竟它可能比人类更懂 LaTeX。

## 使用方法

在导言区加载主题：

```latex
\documentclass[aspectratio=169, 11pt]{beamer}
\usepackage{njubeamer}

\title{你的标题}
\subtitle{副标题（可选）}
\author{汇报人：XXX \quad 指导老师：XXX}
\institute{专业：XXX \quad 学号：XXXX}

\begin{document}
  \titlepage
  \tableofcontents
  \section{第一章}
  \begin{frame}{页面标题}
    内容
  \end{frame}
\end{document}
```

## 编译

本主题使用 `ctex` 宏包，**必须使用 `xelatex` 编译**。

### 使用 latexmk（推荐）

```bash
latexmk njubeamer-demo.tex
```

`.latexmkrc` 已配置好使用 `xelatex` 自动编译。

### 手动编译

```bash
xelatex njubeamer-demo.tex
xelatex njubeamer-demo.tex   # 第二遍生成目录和交叉引用
```

## 页面样式说明

| 页面 | 说明 |
|------|------|
| **标题页** | 顶部白色横幅 + 紫色校徽校名，紫色大标题，信息栏，底部校训 |
| **目录页** | 左侧紫色竖条（目录 CONTENTS），右侧编号目录项 |
| **章节页** | 淡紫色校徽水印背景，全宽紫色横条白色章节标题 |
| **正文页** | 左上角 16pt 紫色标题 + 下划线，右上角校徽校名，右下角页码 |

## 自定义

### 调整标题字号

在 `njubeamer.sty` 中修改：

```latex
\newcommand{\nju@frametitlesize}{\fontsize{16pt}{20pt}\selectfont}
```

### 调整颜色

在 `njubeamer.sty` 开头修改：

```latex
\definecolor{NJUPurple}{RGB}{92, 0, 92}
```
