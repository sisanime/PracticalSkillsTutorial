---
title: lec4 - 2023春夏实用技能拾遗
separator: <!--s-->
verticalSeparator: <!--v-->
theme: simple
highlightTheme: github
css: custom.css
revealOptions:
    transition: 'slide'
    transitionSpeed: fast
    center: false
    slideNumber: "c/t"
    width: 1000
---

<div class="middle center">
<div style="width: 100%">

<img src="logo.png" style="margin-bottom: 1em">

# lec4：LaTeX 排版简要介绍

<hr/>

2023 年春夏学期计算机学院朋辈辅学「实用工具拾遗」课程

By [@TonyCrane](https://github.com/TonyCrane)

<div style="text-align: right; margin-top: 1em;">
<p>2023.5.21&emsp;&emsp;&emsp;</p>
</div>

</div>
</div>

<!--v-->

## 本节内容

- 什么是 LaTeX？如何编写？如何编译？
- 基础的 LaTeX 语法
    - 段落特点及排版样式
    - 各种文档元素以及常用环境
- LaTeX 数学公式语法
    - 包括数学公式编写规范
- 模版套用

<hr/>

我想让你 take-away 的内容：

- LaTeX 数学公式编写规范

<!--v-->

## 如何自学本章节内容

- 安装：[一份简短的关于 LaTeX 安装的介绍](https://github.com/OsbertWang/install-latex-guide-zh-cn)
- 学习：[一份（不太）简短的 LaTeX2e 介绍](https://github.com/CTeX-org/lshort-zh-cn)（即 lshort）
    - 或安装 TeXLive 后直接通过 texdoc lshort-zh-cn 命令打开
- 其他参考：
    - 符号大全：[The Comprehensive LaTeX Symbol List](https://www.ctan.org/pkg/comprehensive)
    - 手写查询：[Detexify](http://detexify.kirelabs.org/classify.html)
    - [LaTeX Wikibook](https://en.wikibooks.org/wiki/LaTeX)
    - [LaTeX Math Wikibook](https://en.wikibooks.org/wiki/LaTeX/Mathematics)
    - [LaTeX StackExchange](https://tex.stackexchange.com/)
    - [LearnLaTeX.org](https://www.learnlatex.org/en/)
- 本 slide Part.5 Part.6 最后一部分

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.1 什么是 LaTeX？

</div>
</div>

<!--v-->

## 历史？

- 1977 年，Donald Knuth 为了排版《计算机程序设计艺术》而开始开发 $\TeX$
    - ASCII 形式写作 TeX（注意大小写）
    - 1982 年发布，性能高，稳定性好，但编写较复杂
- Lamport 为了简化使用，更专注于内容本身，开发了更高层的封装 $\LaTeX$
    - ASCII 形式写作 LaTeX（注意大小写）
    - 后交给 LaTeX 工作组开发维护
    - 1994 年发布 $\LaTeX2_\varepsilon$，即 LaTeX2e，仍是目前主流版本

优缺点：

- 优点：专注于内容本身，排版效果好，公式排版强大，跨平台开源……
- 缺点：学习成本高，不容易排错，不容易定制样式，不所见即所得……

<!--v-->

## 关于发行版、引擎、编译指令

- 发行版即打包好的安装套装，包含了 LaTeX 引擎、宏包、字体等
    - 推荐 TeXLive（全平台，mac 上即 MacTeX）
    - Windows 还有 MiKTeX
- 引擎即编译器，是编译源代码生成文档的排版引擎
    - 有 TeX、pdfTeX、XeTeX、LuaTeX 等
- 有两种语言的格式，分别为 plainTeX（最初版本）和 LaTeX
- 编译指令即编译源代码生成文档的命令，根据引擎和格式来选择

<style>
.reveal table {
    font-size: 0.6em;
}
</style>

|引擎|目标格式|使用 plainTeX|使用 LaTeX|
|:-:|:-:|:-:|:-:|
|TeX|DVI|tex||
|pdfTeX|DVI|etex|latex|
|pdfTeX|PDF|pdftex|pdflatex|
|XeTeX|PDF|xetex|**xelatex**|
|LuaTeX|PDF|luatex|lualatex|

<!--v-->

## 如何编写/编译 LaTeX 文档

- LaTeX 也是纯文本文件，后缀名为 `.tex`，可以用任何文本编辑器编写
- 可以使用 VSCode 配合 LaTeX Workshop 等插件编写
- 也可以使用 TeXStudio 等专门的 LaTeX 编辑器编写
- 编译文档就是执行上页说到的编译指令并传入文件
    - 例如 `xelatex main.tex`，其中 `main.tex` 为文件名
    - 使用编辑器自动编译的话注意设置编译指令
- 使用 BibTeX 编译参考文献等时可能需要多次编译
    - 可以使用 `latexmk` 工具自动编译
    - 详见 [latexmk 文档](https://mg.readthedocs.io/latexmk.html)
- 更方便一点，可以使用在线编辑器 [Overleaf](https://www.overleaf.com/) 编写

<!--v-->

## Hello World

```latex
\documentclass{article}
\begin{document}
Hello World!
\end{document}
```

- 输入 `xelatex main.tex` 编译，得到 `main.pdf`，即为文档
- 或者 `latexmk -xelatex main.tex` 编译
    - 通过 `latexmk -c` 清理编译过程中的临时文件

<!--v-->

## 中文支持

推荐使用 xelatex 编译（注意使用 UTF-8 编码），有两种方案：

- 使用 ctexart ctexrep ctexbook 等文档类
    ```latex
    \documentclass{ctexart}
    \begin{document}
    你好，世界！
    \end{document}
    ```
- 引入 ctex 宏包
    ```latex
    \documentclass{article}
    \usepackage{ctex}
    \begin{document}
    你好，世界！
    \end{document}
    ```

<!--v-->

## LaTeX 命令、代码结构

- 命令（控制序列）以 `\` 开头，对大小写敏感，如 `\LaTeX` -> $\LaTeX$
- 有些命令会对后续内容产生影响，可以用 `{}` 限定作用范围，如 {\bf bold}
- 命令可以接收参数，[] 中为可选参数，{} 中为必选参数，逗号分隔
- 一组常见的命令是**环境**
    ```latex
    \begin{environment}[optional args]{required args}
    ... 
    \end{environment}
    ```
- 文件结构
    ```latex
    \documentclass{article} % 百分号为注释
    % 导言区，调用宏包、定义命令、进行文档设置等
    \begin{document}
    % 正文
    \end{document} % 后续忽略
    ```


<!--v-->

## 文档类与宏包

- LaTeX 文档开头必须包含 documentclass 指定文档类
    - LaTeX 提供的基础文档类有 article report book 等
    - 可以通过可选参数配置字号、纸张大小等
        ```latex
        \documentclass[11pt,a4paper,twoside]{article}
        ```
- 宏包相当于第三方库，可以引入更丰富的扩展功能
    ```latex
    \usepackage[options]{package}
    \usepackage{package1, package2}
    ```
    - 需要确保已经安装，否则会报错
    - TeXLive full 会自带大部分会用到的宏包
    - `texdoc package` 可以查看宏包文档

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.2 段落及排版样式

</div>  
</div>

<!--v-->

## 段落

- 源代码中编写文本即为段落，同样需要注意的是**空白字符**的问题
- 连续的多个空白字符视为一个空格，行首的空格忽略
- 行尾的换行符视为一个空格（中文语境除外）
- 分段落
    - 空一行或者使用 `\par` 命令
    - 多个空行同样视为一个空行

Try this：

```latex
Several spaces     equal one.
   Front spaces are ignored. 中文结尾
不会添加空格。

空行可以换段落。\par
或者使用 \verb|\par| 也可以。
```

<!--v-->

## 断行和断页

- \\\\ 或者 \newline 命令可以手动断行
    - 需要注意这里的断行相当于段落内换行，不会产生新的段落
    - -> 断行的下一行不会缩进
- \newpage 命令可以手动断页
    - 也可以使用 \clearpage

Try：

```latex
This is a paragraph, and I want to break it. \\
This is the next line, but it is not indented.

This is a new paragraph.\newpage
This is the next page.
```

<!--v-->

## 关于字符和标点

- 由于 LaTeX 中使用 \ 开头表示命令，其他符号比如 % 也有其他意义
- 使用这些字符需要通过转义字符来输入
    - `\%` `\&` `\#` `\{` `\}`
    - `\~{}` `\^{}`（带大括号防止将后面的字符理解为参数）
    - `\textbackslash`（即 \，不能使用 \\\\ 因为这是手动换行）
- 一些奇怪的标点符号（仅西文，中文的一般很正常）
    - 单引号 `'，双引号 ``''
    - 连字号 -，短破折号 --，长破折号 ---（EM DASH）
    - 省略号相比三个点更推荐 \ldots
- 防止西文连字
    - 通过 {} 分隔即可避免连字
    ```latex
    difficult dif{}f{}icult
    ```

<!--v-->

## 字体样式和字号

- 两种修改字体样式的命令 {\bfseries bold} \textbf{bold}
    - 常用第二种
    - 都是“内置”字体的不同样式，自定义字体自行查阅
    - 常用的有：\textbf \textit \texttt \textsf \textsc \textsl
- 一种形式的命令来修改字号 {\tiny some text}
    - 从小到大：\tiny \scriptsize \footnotesize \small \normalsize \large \Large \LARGE \huge \Huge

```latex
\textbf{bold} \textit{italic} \texttt{typewriter}
\textsf{sans serif} \textsc{Small Caps} \textsl{slanted}

{\tiny tiny} {\scriptsize scriptsize} {\footnotesize footnotesize}
{\small small} {\normalsize normalsize} {\large large}
{\Large Large} {\LARGE LARGE} {\huge huge} {\Huge Huge}
```

<!--v-->

## 强制行距/间距

- 长度单位：pt，in（英寸），cm，mm，em（当前 M 宽度），ex（当前 x 高度）
- 行距：\linespread{factor}（默认行距 1.2 倍字号大小）
    - 局部修改要在后面加 \selectfont，且要在范围内手动分段
- 插入水平间距（多空格无效）：\hspace{length}
    - \quad 相当于 1em，\qquad 相当于 2em
    - \hspace* 防止因为断行而消失
- 插入垂直间距：\vspace{length}
    - \\\\[1em] 可以在换行的同时插入垂直间距

```latex
{\linespread{2.0}\selectfont This is a paragraph with 2.0 linespread.\\
This is the next line and \quad space and \hspace{3em} more and
\hspace{\fill}lol\par}

\vspace{-1em}Next paragraph.\\[1em] and next line.
```

<!--v-->

## *页边距和分栏

- 通过 geometry 宏包设置页边距
    ```latex
    \usepackage[left=1in,right=1in,top=1in,bottom=1in]{geometry}
    \usepackage[margin=1in]{geometry} % 全相等
    \usepackage[hmargin=1.5in,vmargin=1in]{geometry} % 水平和垂直
    \usepackage[inner=1in,outer=1in]{geometry} % 内侧和外侧
    ```
    ```latex
    \usepackage{geometry}
    \geometry{left=1in,right=1in,top=1in,bottom=1in}
    ```
- 全局分栏 \twocolumn \onecolumn
- 局部分栏 \begin{multicols}{2} \end{multicols}

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.3 文档元素

</div>
</div>

<!--v-->

## 章节和目录

- 应该对文档合理的分割为章、节、小节等，层次依次为
    ```latex
    \chapter{...} \section{...} \subsection{...} \subsubsection{...}
    \paragraph{...} \subparagraph{...}
    ```
    - 其中 \chapter 仅适用于 book 和 report 文档类
    - 可以加入可选参数表示短标题（显示在目录和页眉页脚中）
    - 可以带星号表示不编号（正常会带三级编号）
- 使用 \tableofcontents 生成目录（新的一章/一节）
    - 生成目录需要编译两次
    - 可以通过 \addcontentsline{toc}{*section*}{*name*} 手动添加目录项
- \appendix 之后为附录，编号从 A 开始

<!--v-->

## 生成标题页

- 一般在导言区设置信息
    - \title{...} 设置标题（必需）
    - \author{...} 设置作者（不写会警告），\and 间隔，\thanks 脚注
    - \date{...} 设置日期（不写默认当前日期 \today）
- 通过 \maketitle 生成标题页

```latex
\title{The Title}
\author{TonyCrane\thanks{me@tonycrane.cc}\and 鹤翔万里\thanks{也是我}}
\date{\today}

\begin{document}
\maketitle
...
```

<!--v-->

## 脚注

- \footnote{...} 生成脚注
    - 脚注编号会全局自动排序
- 在表格环境中不能自动正确生成脚注，需要手动
    - \footnotemark 生成脚注编号
    - \footnotetext{...} 生成脚注内容

```latex
some text\footnote{footnote}

\begin{tabular}{l}
\hline some text\footnotemark \\\hline
\end{tabular}
\footnotetext{footnote}
```

<!--v-->

## 图片

- 需要使用 graphicx 宏包 \usepackage{graphicx}
- \includegraphics[options]{filename}
    - options：
        - width=...：宽度；height=...：高度
        - scale=...：缩放比例；angle=...：旋转角度（逆时针）
    - filename：可以是相对路径，也可以是绝对路径
        - 不能包含空格，建议全英文
        - 可以省略后缀名，会自动按照一定顺序搜索
- \graphicspath{{path1}{path2}...} 设置图片搜索路径
- 可以为 graphicx 或文档类设置 draft 选项
    - 显示等大图片框架，而非实际插入图片

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.4 常用环境

</div>
</div>

<!--v-->

## 列表

- itemize 无序列表，enumerate 有序列表
    - \usepackage{enumerate} 可选 [(1)] [i.] [a)] 等指定编号格式
    - \setcounter{enumi}{*i*} 后下一个 item 从 *i*+1 开始编号
- \item 生成列表项，后接内容，\item[...] 可以自定义符号标签
    - 使用 description 环境这里标签会加粗左对齐表示关键字
- 可以嵌套列表，最多四层

```latex
\begin{itemize}
    \item First item
    \item[+] Second item
    \begin{enumerate}[i.]
        \setcounter{enumi}{2}
        \item First subitem
        \item Second subitem
    \end{enumerate}
\end{itemize}
```

<!--v-->

## 对齐环境

- center 居中，flushleft 居左，flushright 居右
    - 这里指的是对齐**环境**
    - 会在环境上下额外生成间距
- \centering \raggedright \raggedleft
    - 注意 raggedright 实际上是左对齐
    - 这里指的是对齐**命令**
    - 不会在环境上下额外生成间距，直接改变对齐方式

```latex
\begin{center}some text\end{center}
\begin{flushright}some text\end{flushright}

\centering some text

\raggedleft some text
```

<!--v-->

## 代码环境

- verbatim 环境
    - \begin{verbatim}...\end{verbatim}
    - 会原样输出，等宽显示，不会解释其中的 LaTeX 命令
- 行内代码可以使用 \verb*\<delim>*...*\<delim>*（一般用 \verb|...|）
    - 也可以使用 \verb*|...|，* 表示显示空格
- listings 宏包可以生成高亮代码，可以自行了解

```latex
\begin{verbatim}
#include <stdio.h>
int main() {
    printf("Hello, world!\n");
    return 0;
}
\end{verbatim}

\verb|\LaTeX| and \verb*|printf("Hello, world!\n");|
```

<!--v-->

## 表格

- 表格使用 tabular 环境，有时可以使用 array 宏包提供辅助
- 直接使用 tabular 环境会和文本混排
    - 一般使用 table 包裹变成浮动体
- \begin{tabular}{cols}，其中列格式 cols：
    - l/c/r：左/中/右对齐
    - |：竖线分隔；@{}：去除列间距；@{...}：自定义列间内容
    - p{width}：指定列宽，自动换行
    - *{num}{col}：重复 num 次 col 列格式
- 内容一行中用 & 分隔，用 \\\\ 换行，\hline 画横线，\cline{*i*-*j*} 部分横线
- booktabs 宏包提供了三线表的线型 \toprule \midrule \bottomrule
- 合并单元格等更多更复杂的功能也支持，可以自行了解
- 推荐使用 [TablesGenerator](https://www.tablesgenerator.com/) 生成（因为真的太难写了）

<!--v-->

## 浮动体

- 使得图片和表格脱离文本，独立寻找合适的位置排放
- 使用 figure 环境包裹图片，table 环境包裹表格
    ```latex
    \begin{figure}[placement]
        ...
    \end{figure}
    ```
    - placement：h 当前位置，t 页面顶部，b 页面底部，p 单独一页，! 忽略限制
    - 默认 tbp，按 h-t-b-p 顺序
    - 限制包括：每页浮动体数量，占页面比例，浮动体间距等
- 内部使用 \caption{...} 添加标题（后可以接 \label{...} 用于引用）
- 可以 \listoftables \listoffigures 生成目录

<!--v-->

## *补：交叉引用

- 使用 \label{*ref*} 添加标签，\ref{*ref*} 引用，\pageref{*ref*} 引用页码
- 可以使用 \label 记录的位置：
    - 章节标题后紧接着使用
    - 行间公式中任意位置使用
    - 有序列表每个 item 中使用
    - 浮动体 caption 后紧接着使用
    - 定理环境内部使用
- 不会记录编号的命令不可以使用（比如 \section*）

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.5 数学公式

</div>
</div>

<!--v-->

## 数学模式

- 编写数学公式首先推荐载入 amsmath 宏包
- 两种排版方式
    - 行内公式，用一对 $ 包裹
    - 行间公式，有几种方法：
        - equation 环境（带编号），\notag 或 equations* 取消编号
        - 一对 $$ 包裹或 <span>\\[</span>...\\] 包裹或 displaymath 环境
        - 其他多行环境 multiline align 等
- 数学模式相比文本模式的不同
    - 字体会发生变化，所以字母当作变量，输入文本需要用 \text{...} 包裹
    - 所有空格都会被忽略，只能通过命令人为引入间距
    - 不允许有空行，除多行环境外不能使用 \\\\ 手动换行


<!--v-->

## 公式排版基础

- 直接写字母就表示变量，\ 开头是命令（命令后面不要加字母，建议加空格）
- 空格均会忽略，手动空格 \\, \\: \\; \ \quad \qquad，\\! 负空格
- 上下标使用 ^ 和 _，超过一个字符需要用 {} 包裹
    - 有些时候也表示子式，比如 \sum \int 等
- 特定函数有命令时要使用专门的命令，比如 \sin \log \lim 等
    - 没有的时候可以用 \mathrm{...} 包裹
- 内部要穿插文字时使用 \text{...} 包裹
    - 不要滥用 \text，文字占多数时考虑分开为多个行内公式
- 有两种样式，\displaystyle 和 \textstyle，即行间和行内
    - 例如 \sum（巨算符）的上下标位置，\int 的高度，\frac 的分数样式等会有不同
    - 也可以使用 \limits 和 \nolimits 改变上下标位置

<!--v-->

## 常用数学符号

- 希腊字母：\alpha $\alpha$ \beta $\beta$ ... \Gamma $\Gamma$ \Delta $\Delta$ ...
- 无穷大 \infty $\infty$；根式 \sqrt{...} n 次根 \sqrt[n]{...}
- 一些省略号 \dots $\dots$ \cdots $\cdots$ \vdots $\vdots$ \ddots $\ddots$
- 分式 <span class="heti-skip">\frac{分子}{分母} \dfrac{分子}{分母} \tfrac{分子}{分母}</span>
- a\bmod b $a\bmod b$；x\equiv a\pmod{b} $x\equiv a\pmod{b}$
- \bar{} \vec{} \hat{} \overline{} \underline{} \widehat{} \overrightarrow{}
- \left( \right) 等自动匹配大小 \left. \right. 取消一侧
- \bigl( \Bigr) \biggl( \Biggr) 等手动大小
- ...
- 更多参考 lshort 或[符号大全](https://www.ctan.org/pkg/comprehensive)
- [LaTeX 数学公式大全 - lowa_BattleShip](https://www.luogu.com.cn/blog/IowaBattleship/latex-gong-shi-tai-quan) 也是一个不错的整合

<!--v-->

## 特殊数学字体

- 公式环境中不能使用 \textbf \textit 等命令
- 针对数学环境中的字符有特定的命令

| 命令 | 样式 | 备注 |
| --- | --- | --- |
|\mathrm{...}|$\mathrm{ABCDEabcde1234}$||
|\mathit{...}|$\mathit{ABCDEabcde1234}$||
|\mathbf{...}|$\mathbf{ABCDEabcde1234}$|粗斜体使用 \boldsymbol|
|\mathsf{...}|$\mathsf{ABCDEabcde1234}$||
|\mathtt{...}|$\mathtt{ABCDEabcde1234}$||
|\mathcal{...}|$\mathcal{ABCDE}$|只有大写|
|\mathbb{...}|$\mathbb{ABCDE}$|只有大写，依赖 amssymb|
|\mathfrak{...}|$\mathfrak{ABCDEabcde1234}$|依赖 amssymb|
|\mathscr{...}|$\mathscr{ABCDE}$|只有大写，依赖 mathrsfs|

<!--v-->

## align/aligned 对齐环境

- 最常用 align 环境进行多行公式对齐
- 用 & 分隔/标记对齐位置，用 \\\\ 换行
    ```latex
    \begin{align}
    a &= b + c  &     g &= h + i \\
      &= d + e + f  &   &= j
    \end{align}
    ```
- 用 \notag 取消编号，或使用 align* 环境
- 可以使用 aligned 环境嵌套在其他环境中（如 equation）来提供对齐部分
    - aligned 本身并不会进入数学模式，需要在数学模式中嵌套使用

<!--v-->

## 矩阵环境

- 可以使用内置的 array 环境，用法类似 tabular，左右需手动加括号
- 推荐使用 amsmath 的矩阵环境
    - matrix 不带定界符
    - pmatrix 小括号；bmatrix 中括号；Bmatrix 大括号
    - vmatrix 单竖线；Vmatrix 双竖线
    - 写法同样 & 分隔，\\\\ 换行

```latex
\[
\mathbf{A} = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
a_{n1} & a_{n2} & \cdots & a_{nn}\\
\end{bmatrix}
\]
```

<!--v-->

## 一些需要注意的规范写法

- 特定函数一定要用专门命令，或写为正体
    - ❌ \mathrm{lim}\_{i\to 0} $\displaystyle\mathrm{lim}\_{i\to 0}$ ✅ \lim\_{i\to 0} $\displaystyle\lim\_{i\to 0}$
    - ❌ log N $log N$ ✅ \log N $\log N$
- 正确、合理地选择使用 displaystyle（以及 dfrac）
- 除了变量以外都要用正体，特别是微分算子
    - ❌ \frac{d}{dx} $\frac{d}{dx}$ ✅ \frac{\mathrm d}{\mathrm dx} $\frac{\mathrm{d}}{\mathrm{d}x}$
- 建议在微分算子之前加上 \\, 稍增加间距
    - ❌ \int x\mathrm dx $\int x\mathrm dx$ ✅ \int x\\,\mathrm dx $\int x\\,\mathrm dx$
- 关于 \colon 用法，常用在映射上（标点），: 用在集合中（运算符）
    - ❌ f: A\to B $f: A\to B$ ✅ f\colon A\to B $f\\colon A\\to B$
    - ❌ \\{x\colon x=1\\} $\\{x\colon x=1\\}$ ✅ \\{x:x=1\\} $\\{x:x=1\\}$

<!--v-->

## 一些需要注意的规范写法（续）

- 括号括起来的内容不为一个字符高时要用 \left \right
    - ❌ (x^2)^2 $(x^2)^2$ ✅ \\left(x^2\\right)^2 $\\left(x^2\\right)^2$
- 行间公式上方不可以加空行
    - 不然行间公式算新一段落，会导致上方间距过大
    ```latex
    some text.
    % 这里不可以有空行
    \[a^2+b^2=c^2\]
    ```

推荐一个锻炼写数学公式用的网站：[TeXnique](https://texnique.xyz/)

- 推荐 Zen mode 练习所有题目
- 只要没有通过就是说明写法还是不规范（规范写法结果应该和预期完全一致）

<!--s-->

<div class="middle center">
<div style="width: 100%">

# Part.6 more？

</div>
</div>

<!--v-->

## 使用模版

- GitHub 上有大量的 LaTeX 模板，可以搜索使用
- 一般就是安装/放到文件夹，然后通过 documentclass 使用模版

我用过的一些模板

- beamer：幻灯片模板
    - 官方文档：[beameruserguide.pdf](http://mirrors.ctan.org/macros/latex/contrib/beamer/doc/beameruserguide.pdf)
    - [Overleaf 上的教程](https://www.overleaf.com/learn/latex/Beamer)
- [ElegantLaTeX](https://github.com/ElegantLaTeX) 系列模板
    - 包括 ElegantBook ElegantPaper ElegantNote
    - 比裸着用 LaTeX 要好看很多

<!--v-->

## 更多资料

进一步学习：

- [一份（不太）简短的 LaTeX2e 介绍](https://github.com/CTeX-org/lshort-zh-cn)（lshort）
- [LaTeX 论文写作教程](https://github.com/xinychen/latex-cookbook)（latex-cookbook）

技巧与注意事项：

- [guanyingc/latex_paper_writing_tips](https://github.com/guanyingc/latex_paper_writing_tips)
- [dspinellis/latex-advice](https://github.com/dspinellis/latex-advice)
- [TeXtw/latex-convention](https://github.com/TeXtw/latex-convention)

符号查阅表：

- 符号大全：[The Comprehensive LaTeX Symbol List](https://www.ctan.org/pkg/comprehensive)
- 手写查询：[Detexify](http://detexify.kirelabs.org/classify.html)

<!--s-->

<div class="middle center">
<div style="width: 100%">

# 总结

</div>
</div>

<!--v-->

<div class="middle center">
<div style="width: 100%">

# 谢谢大家

<hr/>

**Questions?**

</div>
</div>