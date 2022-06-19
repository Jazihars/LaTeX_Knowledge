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
接下来我们以简历为例，来进行测试。由于排版英文简历更容易一些，我们在这里测试中文简历的排版。中文简历的排版主要是需要解决一些字体的安装问题。这里我们以Github上的开源简历模板https://github.com/hijiangtao/resume 为例来进行测试。

首先，下载Github上的开源简历模板https://github.com/hijiangtao/resume 的全部代码。

然后，我们在任意路径建立一个自己的文件夹`LaTeX_test`。新建文件LaTeX_test/mytest.tex

接下来，把开源代码https://github.com/hijiangtao/resume 中的`fonts`文件夹和所有`.cls`、`.sty`文件复制到我自己的文件夹`LaTeX_test`之下。

然后把下述代码放入LaTeX_test/mytest.tex文件：
``` tex
% !TEX TS-program = xelatex
% !TEX encoding = UTF-8 Unicode
% !Mode:: "TeX:UTF-8"

\documentclass{resume}
\usepackage{zh_CN-Adobefonts_external} % Simplified Chinese Support using external fonts (./fonts/zh_CN-Adobe/)
%\usepackage{zh_CN-Adobefonts_internal} % Simplified Chinese Support using system fonts
\usepackage{linespacing_fix} % disable extra space before next section
\usepackage{cite}

\begin{document}
\pagenumbering{gobble} % suppress displaying page number

\name{江涛}

% {E-mail}{mobilephone}{homepage}
% be careful of _ in emaill address
\contactInfo{(+86) 132-6029-9910}{hijiangtao@gmail.com}{Web前端研发工程师}{GitHub @hijiangtao}
% {E-mail}{mobilephone}
% keep the last empty braces!
%\contactInfo{xxx@yuanbin.me}{(+86) 131-221-87xxx}{}
 
\section{个人总结}
本人在校成绩优秀、乐观向上，工作负责、自我驱动力强、热爱尝试新事物，认同开放、连接、共享的Web在未来的不可替代性。在校期间长期从事可视分析(Web的2D/3D时空可视化)相关研究，对Web技术发展趋势及前端工程化解决方案有浓厚兴趣。\textbf{现任职于阿里巴巴集团。}

% \section{\faGraduationCap\ 教育背景}
\section{教育背景}
\datedsubsection{\textbf{中国科学院大学},计算机应用技术,\textit{在读硕士研究生}}{2015.9 - 2018.6}
\ \textbf{排名11/133(前10\%)},中国科学院大学学业奖学金(2次),IEEE Student member,预计2018年6月毕业
\datedsubsection{\textbf{北京理工大学},软件工程,\textit{工学学士}}{2011.9 - 2015.6}
\ \textbf{排名2/62(前5\%)},国家励志奖学金,人民奖学金(7次),科技竞赛奖(2次),北京市普通高等学校优秀毕业生,北京理工大学优秀毕业生,软件学院金牌毕业生,优秀团员/优秀学生(5次)
\datedsubsection{\textbf{荷兰 莱顿大学},计算机科学与技术,\textit{国家留学基金委公派交换生}}{2015.3 - 2015.5}
\ 2014年中国政府奖学金(\textit{http://www.csc.edu.cn/}),DID-ACTE项目交换生(\textit{http://did-acte.org/})

% \section{\faCogs\ IT 技能}
\section{技术能力}
% increase linespacing [parsep=0.5ex]
\begin{itemize}[parsep=0.2ex]
  \item \textbf{编程语言}: JavaScript (ECMAScript, Node.js), HTML/CSS, Python, Go, SQL, C, Shell
  \item \textbf{操作系统,数据库与工程构建}: Linux/macOS/MySQL/MongoDB/Git/webpack/Progressive Web App
  \item \textbf{关键词}: React/Vue.js/D3.js(SVG)/three.js(canvas, WebGL)/chrome extension/Express
\end{itemize}

% \end{itemize}

\section{实习经历}
\datedsubsection{\textbf{阿里巴巴集团 | Alibaba}, 前端开发工程师}{2017.6-2017.9}
\begin{itemize}
%   \item 飞猪北京前端团队全面负责各交通线的票务(机票/火车票/汽车票) web 应用与事业群基础架构研发
  \item 独立负责车站地图开发(React),通过HTML5 本地存储及JSBridge实现在阿里全系应用中发布上线
  \item 独立负责BU SPM chrome插件开发,支付成功/订单详情等页面的开发与交叉营销的接入工作
\end{itemize}

\datedsubsection{\textbf{北京腾云天下科技有限公司 | TalkingData},数据挖掘与可视化工程师}{2015.11-2017.5}
\begin{itemize}
  \item \textbf{利用海量用户定位数据，对城市空间及人群移动特征进行研究。}第一个课题是基于香农熵和人群出行模式，构建城市网格与用户矩阵分析城市多样性/流动性分布；可视分析平台前端与可视化基于D3/Vue/Express开发，数据分析与存储采用Python/MySQL/MongoDB技术，为了均衡大数据情况下的页面可视化渲染消耗用canvas替代svg。第二个课题是对海量商场定位数据做人群分类与可视化查询，依据该系统撰写的论文被CIKM 2016(DAVA Workshop)录用，并收录于中科院软件所年会成果集
  \item 负责数据科学部HQ LAB的可视化原型开发，主导 TalkingMind 平台系统设计与前端开发
\end{itemize}

\datedsubsection{\textbf{北京格灵深瞳信息技术有限公司 | DeepGlint},Web开发工程师}{2015.7-2015.9}
\begin{itemize}
  \item \textbf{独立负责MUSE部门的可视化组件研发。}与平台研发、设计协作完成 DeepGlint Developer 平台可视化图表组件的集成开发，符合完全定制化渲染、响应式布局与实时更新等特点
  \item 利用 D3+Vue+WebGL(Three.js) 尝试实现三维空间的人群移动可视化
\end{itemize}

% \begin{onehalfspacing}
% \end{onehalfspacing}

% \datedsubsection{\textbf{DID-ACTE} 荷兰莱顿}{2015年3月 - 2015年6月}
% \role{本科毕业设计}{LIACS 交换生}
% 利用结巴分词对中国古文进行分词与词性标注，用已有领域知识训练形成 classifier 并对结果进行调优
% \begin{onehalfspacing}
% \begin{itemize}
%   \item 利用结巴分词对中国古文进行分词与词性标注
%   \item 利用已有领域知识训练形成 classifier, 并用分词结果进行测试反馈
%   \item 尝试不同规则，对 classifier 进行调优
% \end{itemize}
% \end{onehalfspacing}

\section{竞赛获奖/项目作品}
% increase linespacing [parsep=0.5ex]
\begin{itemize}[parsep=0.2ex]
%   \item LeetCodeOJ Solutions, \textit{https://github.com/hijiangtao/LeetCodeOJ}
  \item 第三届中国软件杯大学生软件设计大赛\textbf{全国一等奖}( \textit{http://www.cnsoftbei.com/} ),2014 年8月
  \item 中国机器人大赛创意设计大赛\textbf{全国特等奖}( \textit{http://www.rcccaa.org/} ),2013年8月
%   \item 中国机器人大赛暨Robocup公开赛(武术擂台赛)全国一等奖,2013年10月
  \item 第11届北京理工大学“世纪杯”竞赛学生课外科技作品竞赛\textbf{特等奖},2013年8月
  \item VIS Components for security system, \textit{https://hijiangtao.github.io/ss-vis-component/}
  \item 个人博客：\textit{https://hijiangtao.github.io/}，更多作品见 \textit{https://github.com/hijiangtao}
%   \item 电视节目"爸爸去哪儿"可视化分析展示, \textit{https://hijiangtao.github.io/variety-show-hot-spot-vis/}
\end{itemize}

% \section{\faHeartO\ 项目/作品摘要}
% \section{项目/作品摘要}
% \datedline{\textit{An Integrated Version of Security Monitor Vis System}, https://hijiangtao.github.io/ss-vis-component/ }{}
% \datedline{\textit{Dark-Tech}, https://github.com/hijiangtao/dark-tech/ }{}
% \datedline{\textit{融合社交网络数据挖掘的电视节目可视化分析系统}, https://hijiangtao.github.io/variety-show-hot-spot-vis/}{}
% \datedline{\textit{LeetCodeOJ Solutions}, https://github.com/hijiangtao/LeetCodeOJ}{}
% \datedline{\textit{Info-Vis}, https://github.com/ISCAS-VIS/infovis-ucas}{}


% \section{\faInfo\ 社会实践/其他}
\section{社区参与/实践其他}
% increase linespacing [parsep=0.5ex]
\begin{itemize}[parsep=0.2ex]
  \item 乐于参与开源社区讨论,\textbf{参与翻译 Vue.js, webpack, WebAssembly, Babel 文档，印记中文成员}
  \item 中国科学院大学2016秋季学期可视化与可视分析课程助教，\textit{http://vis.ios.ac.cn/infovis-ucas/}
  \item 未来论坛学生会成员、北理社联新闻信息中心主任、北理工软件学院学生会宣传部副部长(2012-2016)
  \item 2013-2015 北京市共青团“温暖衣冬”志愿者,第九届园博会志愿者,2014 FLL机器人世锦赛志愿者
\end{itemize}

%% Reference
%\newpage
%\bibliographystyle{IEEETran}
%\bibliography{mycite}
\end{document}
```

接下来，点击vscode左侧的Activity Bar，使用`xelatex`编译我们的代码。然后就可以在`LaTeX_test`文件夹下查看我们生成的mytest.pdf文件了。


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