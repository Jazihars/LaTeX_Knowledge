# 使用vscode编写LaTeX
vscode是我们最喜爱的文本编辑器。强大、开源、美观是它无可比拟的优点。本文记录了在Windows 10操作系统上使用vscode编写LaTeX的环境配置过程。我们参考[知乎上的使用VSCode编写LaTeX配置教程](https://zhuanlan.zhihu.com/p/38178015)来进行我们自己的配置。


## 安装TeX Live
这个部分我们主要参考[TeX Live 2022 安装指南【安装 LaTeX】](https://zhuanlan.zhihu.com/p/493412905)，也可以参见[Github上的TeX Live2022安装教程pdf版本](https://github.com/OsbertWang/install-latex-guide-zh-cn)。

首先，在[清华源的TeX Live镜像](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)上下载[texlive2022.iso](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/texlive2022.iso)

下载完成后，我们直接点击进入`texlive2022.iso`文件，然后右键点击`install-tl-windows.bat`文件，以管理员身份运行`install-tl-windows.bat`文件即可。剩下的就是比较长时间的安装过程等待了。注意：一定要有耐心，一定要等到安装完全结束才行。绝对不能中途点`Abort（中止、中断）`。


## 在vscode中安装LaTeX Workshop插件
这一步很容易，直接在vscode中搜索这个插件即可。


## 配置vscode的settings.json文件
在vscode界面下按下`F1`，然后键入`setjson`，点击`Preferences: Open Settings(JSON)`，进入对`settings.json`文件的编辑。在`settings.json`文件中，添加如下的代码（注意：只需在`settings.json`文件的后面将下述代码添加到{}内部即可。注意对齐缩进）：
``` json
{
    "latex-workshop.latex.tools": [
        {
            // 编译工具和命令
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOCFILE%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ],
        },
        {
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
}
```
至此，vscode编写LaTeX的环境就配置好了。可以来编写我们自己的任务了。


## LaTeX编写原则
LaTeX的编写原则遵循一般的程序员通用原则：**不要重复造轮子**。实践原则是：**在任何情况下，我们都应该尽可能地使用优质的开源LaTeX模板代码来编写我们自己的LaTeX文件**。现在是开源代码的时代，我们一定得学会使用优质的开源代码。能够找到优质的开源代码，学会使用优质的开源代码，这就是我们编写程序的规则。首先学会使用优质的开源代码，然后在优质开源代码的基础上慢慢微调，增加我们自己的功能，而不是一开始就闷头楞写，这才是最高效的方式。


## 牛刀小试：排版一份简历
接下来我们以简历为例，来进行测试。由于排版英文简历更容易一些，我们在这里测试中文简历的排版。中文简历的排版主要是需要解决一些字体的安装问题。这里我们以overleaf上的开源简历模板https://www.overleaf.com/latex/templates/chinese-resume-template-zhong-wen-jian-li-mo-ban/fbdypsjmgwbb 为例来进行测试。

首先，下载overleaf上的开源简历模板https://www.overleaf.com/latex/templates/chinese-resume-template-zhong-wen-jian-li-mo-ban/fbdypsjmgwbb 的全部代码。点击Open as Template，再点击左上角的Menu，下载Source压缩文件。这就是全部的代码。

然后，我们在任意路径建立一个自己的文件夹`my_resume`。新建文件`my_resume/my_resume.tex`

接下来，解压刚刚下载的压缩文件，把里面的全部内容复制到我们自己的文件夹`my_resume`里面。

然后把下述代码放入`my_resume/my_resume.tex`文件：
``` tex
\documentclass{resume}
\usepackage{zh_CN-Adobefonts_external} 
\usepackage{linespacing_fix}
\usepackage{cite}
\begin{document}
\pagenumbering{gobble}



%***"%"后面的所有内容是注释而非代码，不会输出到最后的PDF中
%***使用本模板，只需要参照输出的PDF，在本文档的相应位置做简单替换即可
%***修改之后，输出更新后的PDF，只需要点击Overleaf中的“Recompile”按钮即可
%**********************************姓名********************************************
\name{Tony Yin}
%**********************************联系信息****************************************
%第一个括号里写手机号，第二个写邮箱
\contactInfo{(+86) XXXXXXXXXXX}{XXXXXXX@XXXXXXXXXX.com}
%**********************************其他信息****************************************
%在大括号内填写其他信息，最多填写4个，但是如果选择不填信息，
%那么大括号必须空着不写，而不能删除大括号。
%\otherInfo后面的四个大括号里的所有信息都会在一行输出
%如果想要写两行，那就用两次这个指令（\otherInfo{}{}{}{}）即可
\otherInfo{性别：男}{籍贯：美国}{}{}
% \otherInfo{来历：钱钟书《围城》}{}{}{}
%*********************************照片**********************************************
%照片需要放到images文件夹下，名字必须是you.jpg，如果不需要照片可以不添加此行命令
%0.15的意思是，照片的宽度是页面宽度的0.15倍，调整大小，避免遮挡文字
\yourphoto{0.13}
%**********************************正文**********************************************


%***大标题，下面有横线做分割
%***一般的标题有：教育背景，实习（项目）经历，工作经历，自我评价，求职意向，等等
\section{教育背景}


%***********一行子标题**************
%***第一个大括号里的内容向左对齐，第二个大括号里的内容向右对齐
%***\textbf{}括号里的字是粗体，\textit{}括号里的字是斜体
\datedsubsection{\textbf{野兔理工学院}，数学学院，\textit{博士}}{2010.09 - 2013.06}


%***********列举*********************
%***可添加多个\item，得到多个列举项，类似的也可以用\textbf{}、\textit{}做强调
\begin{itemize} [parsep=1ex]
    \item \textbf{基础数学}：博士
    \item \textbf{深度学习}：XXX
\end{itemize}

~\\

\datedsubsection{\textbf{野兔理工学院}，计算机学院，\textit{本科}}{2005.09 - 2010.06}
\begin{itemize} [parsep=1ex]
    \item \textbf{计算机科学与技术}：理学学士
\end{itemize}


~\\
~\\
~\\
~\\
~\\
~\\
~\\

\section{职业经历}

\datedsubsection{\textbf{野兔理工技术研发有限公司}，算法实习生}{2011.04 - 2012.06}
\begin{itemize}[parsep=0.5ex]
    \item XXX
    \item XXX
    \item XXX
    \item XXX
\end{itemize}

% \datedsubsection{\textbf{三闾大学}，副教授}{1931.09 - 1934.6}
% \begin{itemize}[parsep=0.5ex]
%     \item 同事：李梅、顾尔、孙、楣
% \end{itemize}

% \datedsubsection{\textbf{上海某报社}，职员}{1934.09 -}
% \begin{itemize}[parsep=0.5ex]
%     \item 生活不如意
% \end{itemize}


~\\
~\\
~\\
~\\
~\\
~\\
~\\


\section{技术能力}

\datedsubsection{\textbf{编程语言}：Python, Shell}{}
% \begin{itemize}[parsep=0.5ex]
%     \item \textbf{地点}：上
% \end{itemize}

\datedsubsection{\textbf{操作系统、数据库与工程构建}：Linux, Git}{}
% \begin{itemize}[parsep=0.5ex]
%     \item \textbf{地点}：上海
%     \item 苏
%     \item 方
% \end{itemize}

% \datedsubsection{\textbf{唐}，热}{1930.08 - 1931.08}
% \begin{itemize}[parsep=0.5ex]
%     \item \textbf{地点}：上海
%     \item 小
%     \item 小
% \end{itemize}

% \datedsubsection{\textbf{孙}，子}{1934.08 -}
% \begin{itemize}[parsep=0.5ex]
%     \item \textbf{地点}：大学
%     \item 讽刺
%     \item 矛盾
% \end{itemize}

% \section{简历写作注意事项}

% 写作时不要泛泛而谈太笼统，要应用STAR原则，即Situation（情景）、Task（任务）、Action（行动）和Result（结果）四个英文单词的首字母组合。

% \begin{itemize}[parsep=0.5ex]
%     \item S指的是situation，事情是在什么情况下发生
%     \item T指的是task，你是如何明确你的目标的
%     \item A指的是action，针对这样的情况分析，你采用了什么行动方式
%     \item R指的是result，结果怎样，在这样的情况下你学习到了什么
% \end{itemize}

\end{document}

```

接下来，点击vscode左侧的Activity Bar，使用`xelatex`编译我们的代码。然后就可以在`my_resume`文件夹下查看我们生成的`my_resume.pdf`文件了。


## 另一个例子：排版毕业论文
我们以清华大学毕业论文模板为例，来演示如何排版毕业论文。这部分内容参见[清华大学自己发布的教程](https://stu.cs.tsinghua.edu.cn/~harry/latex-talk.pdf)。注意：清华发布的这份教程非常有价值，强烈建议学习！

首先，下载[清华大学毕业论文模板所需的宏包](https://github.com/tuna/thuthesis/releases/download/v7.3.0/thuthesis-v7.3.0.zip)

然后，解压这个压缩文件，将这个压缩文件中的所有文件复制到我自己的文件夹`my_phd_thesis`之下。在我自己的文件夹`my_phd_thesis`之下，新建一个`my_phd_thesis.tex`的文件。在`my_phd_thesis.tex`文件中键入如下的代码：
``` tex
% !TeX encoding = UTF-8
% !TeX program = xelatex
% !TeX spellcheck = en_US

\documentclass[degree=master]{thuthesis}
  % 学位 degree:
  %   doctor | master | bachelor | postdoc
  % 学位类型 degree-type:
  %   academic（默认）| professional
  % 语言 language
  %   chinese（默认）| english
  % 字体库 fontset
  %   windows | mac | fandol | ubuntu
  % 建议终版使用 Windows 平台的字体编译


% 论文基本配置，加载宏包等全局配置
\input{thusetup}


\begin{document}

% 封面
\maketitle

% 学位论文指导小组、公开评阅人和答辩委员会名单
% 本科生不需要
\input{data/committee}

% 使用授权的说明
\copyrightpage
% 将签字扫描后授权文件 scan-copyright.pdf 替换原始页面
% \copyrightpage[file=scan-copyright.pdf]

\frontmatter
\input{data/abstract}

% 目录
\tableofcontents

% 插图和附表清单
% 本科生的插图索引和表格索引需要移至正文之后、参考文献前
% \listoffiguresandtables  % 插图和附表清单（仅限研究生）
\listoffigures           % 插图清单
\listoftables            % 附表清单

% 符号对照表
\input{data/denotation}


% 正文部分
\mainmatter
\input{data/chap01}
\input{data/chap02}
\input{data/chap03}
\input{data/chap04}


% 其他部分
\backmatter

% 参考文献
\bibliography{ref/refs}  % 参考文献使用 BibTeX 编译
% \printbibliography       % 参考文献使用 BibLaTeX 编译

% 附录
% 本科生需要将附录放到声明之后，个人简历之前
\appendix
% \input{data/appendix-survey}       % 本科生：外文资料的调研阅读报告
% \input{data/appendix-translation}  % 本科生：外文资料的书面翻译
\input{data/appendix}

% 致谢
\input{data/acknowledgements}

% 声明
\statement
% 将签字扫描后的声明文件 scan-statement.pdf 替换原始页面
% \statement[file=scan-statement.pdf]
% 本科生编译生成的声明页默认不加页脚，插入扫描版时再补上；
% 研究生编译生成时有页眉页脚，插入扫描版时不再重复。
% 也可以手动控制是否加页眉页脚
% \statement[page-style=empty]
% \statement[file=scan-statement.pdf, page-style=plain]

% 个人简历、在学期间完成的相关学术成果
% 本科生可以附个人简历，也可以不附个人简历
\input{data/resume}

% 指导教师/指导小组学术评语
% 本科生不需要
\input{data/comments}

% 答辩委员会决议书
% 本科生不需要
\input{data/resolution}

% 本科生的综合论文训练记录表（扫描版）
% \record{file=scan-record.pdf}

\end{document}

```
然后，在vscode的`settings.json`文件中加入下述代码（这段代码来自清华模板中的.vscode文件夹）：
``` json
{
    "latex-workshop.latex.recipes": [
        {
            "name": "latexmk (xelatex)",
            "tools": [
                "unpack-thuthesis",
                "xelatexmk"
            ]
        },
        {
            "name": "latexmk (lualatex)",
            "tools": [
                "unpack-thuthesis",
                "lualatexmk"
            ]
        },
        {
            "name": "make thesis",
            "tools": [
                "make-thesis"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "unpack-thuthesis",
            "command": "xetex",
            "args": [
                "-file-line-error",
                "-halt-on-error",
                "-interaction=nonstopmode",
                "thuthesis.ins"
            ],
            "env": {}
        },
        {
            "name": "xelatexmk",
            "command": "latexmk",
            "args": [
                "-xelatex",
                "-file-line-error",
                "-halt-on-error",
                "-interaction=nonstopmode",
                "-synctex=1",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "lualatexmk",
            "command": "latexmk",
            "args": [
                "-lualatex",
                "-file-line-error",
                "-halt-on-error",
                "-interaction=nonstopmode",
                "-synctex=1",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
            "env": {}
        },
        {
            "name": "make-thesis",
            "command": "make",
            "args": [],
            "env": {}
        }
    ],
    "latex-workshop.latex.watch.files.ignore": [
        "**/thuthesis.cls",
        "**/*.bbx",
        "**/*.bbl",
        "**/*.cbx",
        "**/*.cfg",
        "**/*.clo",
        "**/*.cnf",
        "**/*.def",
        "**/*.dfu",
        "**/*.enc",
        "**/*.fd",
        "**/*.fmt",
        "**/*.lbx",
        "**/*.map",
        "**/*.mkii",
        "**/*.pfb",
        "**/*.tfm",
        "**/*.vf",
        "**/*.code.tex",
        "**/*.sty",
        "**/texmf-{dist,var}/**",
        "**/Local/MiKTeX/**",
        "**/Local/Programs/MiKTeX/**",
        "**/Roaming/MiKTeX/**",
        "**/Program*/MiKTeX*/**",
        "**/.miktex/texmfs/**",
        "/var/cache/miktex-texmf/**",
        "/usr/local/share/miktex-texmf/**",
        "**/Library/Application Support/MiKTeX/texmfs/**",
        "/dev/null"
    ],
}
```
然后，我们修改清华的模板，来适用于我们自己的学校。比如，我们可以使用vscode打开`my_phd_thesis`文件夹，把`my_phd_thesis`文件夹里所有的`清华大学`修改为`野兔理工学院`，`Tsinghua University`修改为`YeTu Institute of Technology`，就可以适用于我们自己的学校了。


---
这就是我们如何使用vscode和LaTeX来编写简历和毕业论文的简易教程。