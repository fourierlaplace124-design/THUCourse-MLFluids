# 流体力学与人工智能

简洁、学术、按讲次组织的课程笔记模板。使用 **XeLaTeX** 编译，所有源文件均为 UTF-8。
包含中文、数学公式、定理与证明、定义、例题、备注、代码块、图片、三线表、目录及可点击交叉引用。

## 文件结构

```text
.
├── main.tex                     # 入口：课程信息、目录、各讲顺序
├── notes.sty                    # 集中管理样式、环境与常用宏
├── lectures/
│   └── 01-introduction.tex       # 可替换的课程示例
├── figures/
│   └── .gitkeep                 # 保留插图目录；可删除
├── .gitignore                   # 排除编译产物，保留 PDF 插图
└── README.md
```

## 编译

安装包含中文支持的 TeX Live、MacTeX 或 MiKTeX。模板使用发行版提供的 **Fandol** 字体，
无需配置宋体、黑体等系统字体。精简安装需补齐 `ctex`、`xeCJK`（含 `xeCJK-listings`）、
`fandol`、`geometry`、AMS 数学宏包、`bm`、`graphics`、`tools`、`booktabs`、`xcolor`、
`listings`、`hyperref`、`bookmark`；使用下方推荐命令还需 `latexmk`。

在 `main.tex` 所在目录运行：

```sh
latexmk -xelatex -interaction=nonstopmode -halt-on-error -outdir=build main.tex
```

生成的文件为 `build/main.pdf`。`latexmk` 会自动重复编译，使目录和引用稳定。
若没有 `latexmk`，运行两遍：

```sh
xelatex -interaction=nonstopmode -halt-on-error main.tex
xelatex -interaction=nonstopmode -halt-on-error main.tex
```

此时生成 `main.pdf`；如仍提示引用需要更新，再编译一次。
上传至 Overleaf 时保留目录结构，将编译器设为 **XeLaTeX**、主文件设为 `main.tex`。
本模板按 XeLaTeX 配置，不使用 pdfLaTeX；代码高亮不需要 `-shell-escape`。

## 日常记录

1. 在 `main.tex` 中修改课程名、姓名和学期。
2. 用实际课程内容替换 `lectures/01-introduction.tex`。
3. 新建下一讲，例如 `lectures/02-next-topic.tex`；文件只写正文，不重复声明文档类。
4. 在 `main.tex` 中加入 `\input{lectures/02-next-topic}`，按讲课顺序排列。
5. 图片放入 `figures/`，统一在 `notes.sty` 中修改样式与宏。

每讲使用一个 `\section{标题}`，小节使用 `\subsection{标题}`。
新增章节只需添加内容文件和一行 `\input`，无需复制导言区。

## 常用写法

### 数学与陈述

```latex
\begin{definition}[概念名称]\label{def:concept}
定义的内容及适用范围。
\end{definition}

\begin{theorem}[定理名称]\label{thm:result}
假设条件与结论。
\end{theorem}
\begin{proof}
证明过程。
\end{proof}

\begin{example}[例题名称]
题目与解答。
\end{example}
\begin{remark}
易错点、解释或补充。
\end{remark}

\begin{equation}\label{eq:integral}
  I = \int_0^1 x^2\dd x = \frac{1}{3}.
\end{equation}
由定理~\ref{thm:result} 和公式~\eqref{eq:integral} 可知……
```

还可使用 `lemma`（引理）、`proposition`（命题）、`corollary`（推论）。
这些环境与定义、例题、备注共享按节重置的编号，例如“定义 1.1”“定理 1.2”。
公式、图、表各自按节编号；代码块单独连续编号。

| 宏 | 用途 |
| --- | --- |
| `\R`、`\N` | 实数集、自然数集 |
| `\vect{u}` | 粗体向量（也支持希腊字母） |
| `\dd` | 积分中的直立微分符号 |
| `\diag`、`\rank` | 对角矩阵、秩等算子 |

不要覆盖 `\div`、`\d` 等 LaTeX 已有命令。新记号集中放入 `notes.sty`，并在正文解释含义。

### 代码块

```latex
\begin{lstlisting}[language=Python,caption={算法名称},label={lst:algorithm}]
# 中文注释
values = [1, 2, 3]
print(sum(values))
\end{lstlisting}
```

也可使用 `\lstinputlisting[language=Python]{code/example.py}` 读取自己添加的代码文件。
支持常用中文注释；代码仅作排版，不会执行。文件名建议使用小写英文字母、数字和连字符。

### 图片、表格与链接

```latex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.7\linewidth]{my-figure.pdf}
  \caption{图的说明}
  \label{fig:my-figure}
\end{figure}

\begin{table}[htbp]
  \centering
  \caption{结果比较}
  \label{tab:results}
  \begin{tabular}{lc}
    \toprule
    方法 & 误差 \\
    \midrule
    方法 A & 0.01 \\
    方法 B & 0.02 \\
    \bottomrule
  \end{tabular}
\end{table}

见图~\ref{fig:my-figure}、表~\ref{tab:results}。
\href{https://ctan.org/pkg/ctex}{CTeX 文档}
```

图片默认从 `figures/` 查找，支持 PDF、PNG 和 JPG。添加图片后再引用对应文件名。
示例中的占位图可在没有图片时正常编译；放入 `figures/velocity-field.pdf` 后会自动显示该图。
图表允许浮动，以避免大块空白；将 `\label` 放在 `\caption` 后，确保引用正确。
目录及引用均可点击，打印时不显示彩色边框。

## 长期维护

- 将标题、正文、样式分别留在 `main.tex`、`lectures/`、`notes.sty`。
- 给标签加语义前缀，如 `sec:`、`eq:`、`thm:`、`fig:`、`tab:`；整个项目内保持唯一。
- 每次增加内容后编译一次；遇到错误，优先处理日志中的第一个错误。
- 仅提交源文件与必要插图，编译产物已由 `.gitignore` 排除。
- 示例仅演示笔记写法；正式记录时补充教材、课件页码及引用来源。

## 参考文档

- [CTeX：中文文档类与排版](https://ctan.org/pkg/ctex)
- [latexmk：自动重复编译](https://ctan.org/pkg/latexmk)
- [listings：代码排版](https://ctan.org/pkg/listings)
