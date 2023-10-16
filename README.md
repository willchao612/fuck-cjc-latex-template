### 计算机学报 LaTeX 排版碰到的所有问题

> 感谢 [@DeroMadrid](https://github.com/DeroMadrid) 师兄的前期努力！

#### 写在前面 & TLDR

能劝一个是一个：珍爱生命，远离他们家的 LaTeX 板子。简直又陈旧又难用，建议趁早用 Word 排版及时止损。如果你和我们一样不甘心从头再来，那就看下去。

目前网上不存在完全符合该期刊排版要求的 Overleaf 版本，以下方法同样只适用于 Windows 本地编译，环境配置参考 [知乎](https://zhuanlan.zhihu.com/p/166523064)。Mac 和 Linux 都别想了，别问我怎么知道的。

所有修改都只针对 `CjC_template_tex.tex` 文件，可以参考 `fuck-cjc-latex-template.tex` [文件内容](./fuck-cjc-latex-template.tex) 或直接替换原文件进行编译。编译结果即为 [模版](./fuck-cjc-latex-template.pdf)，已通过测试。

（听说录用了要提交 Word 和 PDF 两稿，我现在有点难过）

------

#### 乱码 & 字体

- 修改编码并使用 `zhmetrics` 补全 TeX Live 残缺的字体，详细步骤烦请移步 [B 站专栏 ](https://www.bilibili.com/read/cv20507474/)
- 不要使用 `xelatex` 或 `bibtex`，使用 `pdflatex` 编译两次

#### 摘要

正文前可能出问题的代码修改如下

```latex
\begin{table*}
  [!t]
  \vspace{-13mm}
  \begin{tabular}{p{168mm}}
    \zihao{5-}\begin{CJK*}{UTF8}{song} 第??卷\quad 第?期 \hfill 计\quad 算\quad 机\quad 学\quad 报\hfill Vol. ?? No. ?\end{CJK*} \\
    \zihao{5-}\begin{CJK*}{UTF8}{song} 20??年?月 \hfill CHINESE JOURNAL OF COMPUTERS \hfill ???. 20??\end{CJK*} \\
    \hline
    \\[-4.5mm]
    \hline
  \end{tabular}

  \centering
  \vspace{11mm}
  \begin{CJK} {UTF8}{hei} {\zihao{2} 标题 } \end{CJK}
  \vskip 5mm
  {\zihao{3}\begin{CJK*}{UTF8}{fs} 作者\end{CJK*}}
  \vspace{5mm}
  \zihao{6}{\begin{CJK*}{UTF8}{song} (学校)\end{CJK*}}
  \vskip 5mm {\centering \begin{tabular}{p{160mm}}\zihao{5-}{ \setlength{\baselineskip}{16pt}\selectfont{ \noindent\begin{CJK*}{UTF8}{hei}摘\quad 要\quad\end{CJK*} \begin{CJK*}{UTF8}{song} 摘要内容\end{CJK*}\par}} \\[2mm]
  \zihao{5-}{\noindent \begin{CJK*}{UTF8}{hei}关键词\end{CJK*} \quad \begin{CJK*}{UTF8}{song}{关键词1；关键词2 }\end{CJK*} } \\[2mm] \zihao{5-}{\begin{CJK*}{UTF8}{hei}中图法分类号\end{CJK*} \begin{CJK*}{UTF8}{song} TP\end{CJK*}\rm{\quad \quad \quad } \begin{CJK*}{UTF8}{hei}DOI号:\end{CJK*}\begin{CJK*}{UTF8}{song} *投稿时不提供DOI号\end{CJK*}}\end{tabular}}

  \vskip 7mm
  \begin{center}
    \zihao{3}{ {\begin{CJK*}{UTF8}{hei}{\bf Title}\end{CJK*}}}\\
    \vspace{5mm}
    \zihao{5}{ {\begin{CJK*}{UTF8}{hei}Names\end{CJK*} }}\\
    \vspace{2mm}
    \zihao{6}{\begin{CJK*}{UTF8}{hei}{(School) }\end{CJK*}}
  \end{center}

  \begin{tabular}{p{160mm}}
    \zihao{5}{ \setlength{\baselineskip}{18pt}\selectfont{ {\bf Abstract}\quad \begin{CJK*}{UTF8}{hei}\end{CJK*} \par}}

    \setlength{\baselineskip}{18pt}\selectfont{ \zihao{5}{\noindent First part of the abstract }}
  \end{tabular}

  \setlength{\tabcolsep}{2pt}
  \begin{tabular}{p{0.05cm}p{16.15cm}}
    \multicolumn{2}{l}{\rule[4mm]{40mm}{0.1mm}} \\[-3mm]
                                               & \begin{CJK*}{UTF8}{gbsn}
    本课题得到国家自然科学基金...
    第1作者手机号码...
  \end{tabular}
\end{table*}

\clearpage
\begin{strip}
  \begin{center}
    \vspace{-3em}
    \begin{tabular}{p{160mm}}
      \setlength{\baselineskip}{18pt} Second part of the abstract \\
      \vspace{5mm} {\bf Keywords}\quad \begin{CJK*}{UTF8}{hei} Keyword1; Keyword2 \end{CJK*} \\
    \end{tabular}
  \end{center}
\end{strip}

\newpage
\linespread{1.15}
```

##### 摘要导致第一页超页

将摘要手动截断，把第二部分放到一个 `table` 环境里，但是会导致 [第二页内容超页 ](#摘要导致第二页超页)

##### 摘要导致第二页超页

经过谜一般的尝试之后，先用 `clearpage` 再用 `newpage` 命令可以正常显示

##### 奇怪的 `E`

把那个看上去就多余的 `strip` 环境删掉就可以了

#### 恼人的 `??`

通常是由于只使用 `pdflatex` 编译了一次，再编译一次就行

##### 引用 `ref`

###### 顺序

[^]: https://tex.stackexchange.com/questions/245487/question-marks-in-pdf-file-when-referring-to-table

请把 `label` 放在 `caption` 的后面，且一定要有 `caption`

###### 图表中文标题

[^]: https://tex.stackexchange.com/questions/211783/how-to-change-figure-label-fig-n-instead-figure-n

```latex
\makeatletter
\renewcommand{\fnum@figure}{图 \thefigure}
\renewcommand{\fnum@table}{表 \thetable}
\makeatother
```

##### 文献 `cite`

- 只能用 `bibitem` 一个一个添加
- 不是纯 `GB/T 7714-2015` 格式，需要删掉代表期刊、会议等的中括号
- 使用 `upcite` 而不是 `cite` 才是角标形式

#### 脚注

##### 以阿拉伯数字计数而不是特殊符号

[^]: https://blog.csdn.net/weixin_45176132/article/details/123474293

```latex
\renewcommand{\thefootnote}{\arabic{footnote}}
```

##### 跑到下一页

[^]: https://tex.stackexchange.com/questions/32208/footnote-runs-onto-second-page

```latex
\interfootnotelinepenalty=10000
```

##### 重新计数

```latex
\setcounter{footnote}{0}
```

#### 子图重新计数

```latex
\setcounter{subfigure}{0}
```

#### 与此板子无关的小技巧

##### 图片独占单栏剩余太多空白

[^]: https://blog.csdn.net/qq_42031694/article/details/131248220?spm=1001.2014.3001.5502

```latex
\renewcommand\floatpagefraction{.9}
\renewcommand\topfraction{.9}
\renewcommand\bottomfraction{.9}
\renewcommand\textfraction{.1}
\setcounter{totalnumber}{50}
\setcounter{topnumber}{50}
\setcounter{bottomnumber}{50}
```

##### 表格自适应宽度

[^]: https://tex.stackexchange.com/questions/44795/automatically-stretch-table-to-evenly-fill-horizontal-space

```latex
\begin{table*}
	[!htb] \footnotesize
	\centering
	\caption{文本验证码多步攻击方法}
	\label{text-attack-method-multi}
	\begin{tabularx}
		{\textwidth}{@{\extracolsep{\stretch{1}}}*{5}{c}} \toprule
		\makebox[0.05\textwidth][c]{攻击方法}
		&
		\makebox[0.1\textwidth][c]{预处理}
		&
		\makebox[0.1\textwidth][c]{字符分割}
		&
		\makebox[0.1\textwidth][c]{字符识别}
		&
		\makebox[0.05\textwidth][c]{攻击成功率}
		\\ \bottomrule
	\end{tabularx}
\end{table*}
```

