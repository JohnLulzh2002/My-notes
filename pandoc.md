Pandoc 程序的命令使用方式为：

>  pandoc <files> <options>

其中 <files> 为输入的内容，其输入即可以来自文件，也可以来自标准输入甚至网页链接。而 <options> 为参数选项。主要的参数选项有：

*-f <format>、-r <format>：指定输入文件格式，默认为 Markdown；*

*-t <format>、-w <format>：指定输出文件格式，默认为 HTML；*

*-o <file>：指定输出文件，该项缺省时，将输出到标准输出；*

*--highlight-style <style>：设置代码高亮主题，默认为 pygments；*

*-s：生成有头尾的独立文件（HTML，LaTeX，TEI 或 RTF）；*

*-S：聪明模式，根据文件判断其格式；*

*--self-contained：生成自包含的文件，仅在输出 HTML 文档时有效；*

*--verbose：开启 Verbose 模式，用于 Debug；*

*--list-input-formats：列出支持的输入格式；*

*--list-output-formats：列出支持的输出格式；*

*--list-extensions：列出支持的 Markdown 扩展方案；*

*--list-highlight-languages：列出支持代码高亮的编程语言；*

*--list-highlight-styles：列出支持的代码高亮主题；*

*-v、--version：显示程序的版本号；*

*-h、--help：显示程序的帮助信息。*

虽然 Pandoc 提供了用于指定输入输出格式的参数，但是很多时候该参数不必使用。Pandoc 已经足够聪明到可以根据文件名判断输入输出格式，所以除非文件名可能造成歧义，否则这两个参数都可以省略。

---

[使用示例](http://gnss.help/2017/06/12/pandoc-install-usage/#使用示例)

[信息查看](http://gnss.help/2017/06/12/pandoc-install-usage/#信息查看)

查看程序支持的输入文件格式：

> $ pandoc --list-input-formats

查看程序支持代码高亮的编程语言：

>  pandoc --list-highlight-languages

查看程序帮助：

>  pandoc --help

[生成 HTML 文档](http://gnss.help/2017/06/12/pandoc-install-usage/#生成-HTML-文档)

使用 Pandoc 可以很容易地将 Markdown 文档渲染为 HTML 网页：

>  pandoc demo.md -o demo.html

上面的命令将输出一个 HTML 文档，但该文档不包含任何样式，它的显示效果依赖于你使用的浏览器。我们当然希望可以得到排版更精美的文档，只要在转换时引入自己的层叠样式表 CSS 文件。输入的 CSS 文件可使用 -c 命令来指定：

>  pandoc demo.md -c style.css -o demo.html

如此输出的 HTML 文档已经包含样式文档了，平时自己查看时，效果很不错。但该方式依然存在部分问题。我们发布或共享文档时，需要传送至少两个文件：1 个 HTML 文件和 1 个 CSS 文件，略有些不便。而如果文档中还包含多个本地图片等文件，共享文档几乎成了不可能的事情。好在 Pandoc 可以将外部文件嵌入到 HTML 文档中，生成一个自包含的独立文件：

>  pandoc demo.md --self-contained -c style.css -o demo.html

在该命令中，--self-contained 参数指定：将任何的外部文件嵌入至输出的文件中，形成一个独立的 HTML 文档。这样传送资料时只传送一个文件就可以了，就像分享 PDF 文档一样方便。

[生成 docx 文档](http://gnss.help/2017/06/12/pandoc-install-usage/#生成-docx-文档)

虽然我很喜欢使用 HTML 作为文档交换格式，但某些情况下你可能还是需要传送 Word docx 文件。这也不是问题，Pandoc 能够将所支持的输入文件一键转换为 Word docx 格式。

下面的命令将一份 Markdown 文件转换为 docx 格式：

>  pandoc demo.md -o demo.docx

下面的命令将 HTML 网页转换为 docx 格式：

>  pandoc http://gnss.help/2017/06/12/pandoc-install-usage/ -o this_page.docx

需补充的是：Pandoc 无法为生成的 Word docx 文档指定排版方式。你可能需要二次编辑输出的文件，将标题、正文等调整为满意的样式。

[生成 PDF 文档](http://gnss.help/2017/06/12/pandoc-install-usage/#生成-PDF-文档)

使用 Pandoc 直接生成 PDF 文件时，需要安装 LaTeX。并且，Pandoc 自带的 PDF 引擎不支持中文，必须为中文配置额外的引擎和模板。Pandoc 程序生成 PDF 文件的命令为：

>  pandoc demo.md -o demo.pdf

我生成 PDF 文档时，未使用以上的方法。而是采用 HTML 文件作为中间文件过渡，使用 Windows 系统的 “打印到 PDF” 功能，将 HTML 文档进一步转换为所需的 PDF 文档。

[生成 Markdown 文档](http://gnss.help/2017/06/12/pandoc-install-usage/#生成-Markdown-文档)

别忘了 Markdown 也是 Pandoc 支持的输出格式之一，我们可以将任何支持的输入格式转换为 Markdown。这对于我们将之前的文档也切换到 Markdown 格式来说，实在是太方便了。

下面的命令由 Word docx 文档生成 Markdown 文件：

>  pandoc demo.docx -o demo.md

下面的命令由 HTML 网页生成 Markdown 文档：

>  pandoc http://gnss.help/2017/06/12/pand
>
>  层叠样式表文件决定最终的显示样式，因此有一个漂亮的 CSS 样式表文件非常重要。在此推荐两个 CSS 文件，首先是由 Alberto Leal 制作的 [Github 风格的样式表文件](https://gist.github.com/Dashed/6714393)，它的显示效果类似于 Github 网站的 README 文档。另一个是我制作的，类似本站曾采用的 [Minos](http://blog.zhangruipeng.me/hexo-theme-minos/) 主题（[Minos-style](https://gist.github.com/purpleskyfall/98ecbccf4f2184aa0f365fbbae36ebdd)）的样式表文件。该文件还未完全稳定，尚需部分完善，不过对付一般的文字排版已经没有问题。
