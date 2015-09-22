**特别说明 ：**
本文前半部分改编自[Markdown 語法說明][markdown-doc-tw]，英文版请参考[Markdown: Syntax][markdown-doc-en]。
本站博客系统使用的插件Jetpack支持**PHP Markdown Extra**，可参考[php Markdown Extra][php-markdown-extra-doc-en]。

[markdown-doc-tw]:http://markdown.tw
[markdown-doc-en]:http://daringfireball.net/projects/markdown/syntax
[php-markdown-extra-doc-zh]:http://b.soont.com/md/php-markdown
[php-markdown-extra-doc-en]:https://michelf.ca/projects/php-markdown/extra

-------------------

目录
================

*   [概述](#overview)
    *   [哲学](#philosophy)
    *   [内嵌HTML](#html)
    *   [特殊字符自动转换](#autoescape)
*   [区块元素](#block)
    *   [段落和换行](#p)
    *   [标题](#header)
    *   [区块引用](#blockquote)
    *   [清单](#list)
    *   [程序代码区块](#precode)
    *   [分隔线](#hr)
*   [区段元素](#span)
    *   [链接](#link)
    *   [强调](#em)
    *   [程序代码](#code)
    *   [图片](#img)
*   [其它](#misc)
    *   [转义字符](#backslash)
    *   [自动链接](#autolink)
*   [Markdown扩展](#extra)
    *   [Markdown内嵌HTML](#inline-html)
    *   [HTML内嵌Markdown](#markdown-in-html)
    *   [特殊属性](#special-attribute)
    *   [代码块](#fenced-code-blocks)
    *   [表格](#tables)
    *   [定义列表](#defination-lists)
    *   [脚注](#footnotes)
    *   [缩略短语](#abbreviations)
    *   [强调](#extra-emphasis)
    *   [新增的转义字符](#backslash-escapes)
*   [参考资料](#reference)

**注意：**这份文件是用 Markdown 写的，你可以[看看它的原始档][this_file] 。

[this_file]:https://github.com/hellossd/doc/blob/master/markdown.md

* * *

<h2 id="overview">概述</h2>

<h3 id="philosophy">哲学</h3>

Markdown 的目标是实现「易读易写」。

不过最需要强调的便是它的可读性。一份使用 Markdown 格式撰写的文件应该可以直接以纯文本发布，并且看起来不会像是由许多卷标或是格式指令所构成。Markdown 语法受到一些既有 text-to-HTML 格式的影响，包括 [Setext][1]、[atx][2]、[Textile][3]、[reStructuredText][4]、[Grutatext][5] 和 [EtText][6]，然而最大灵感来源其实是纯文本的电子邮件格式。

  [1]: http://docutils.sourceforge.net/mirror/setext.html
  [2]: http://www.aaronsw.com/2002/atx/
  [3]: http://textism.com/tools/textile/
  [4]: http://docutils.sourceforge.net/rst.html
  [5]: http://www.triptico.com/software/grutatxt.html
  [6]: http://ettext.taint.org/doc/

因此 Markdown 的语法全由标点符号所组成，并经过严谨甄选，是为了让它们看起来就像所要表达的意思。像是在文字两旁加上星号，看起来就像\*强调\*。Markdown 的列表看起来，嗯，就是清单。若你使用过电子邮件，区块引言就真是看起来像引用一段文字。

<h3 id="html">内嵌HTML</h3>

Markdown 的语法有个主要的目的：用来作为一种网络内容的*写作*用语言。

Markdown 不是要来取代 HTML，甚至也没有要和它相似，它的语法种类不多，只和 HTML 的一部分有关系，重点*不是*要创造一种更容易写作 HTML 文件的语法，我认为 HTML 已经很容易写了，Markdown 的重点在于，它能让文件更容易阅读、编写。HTML 是一种*发布*的格式，Markdown 是一种*编写*的格式，因此，Markdown 的格式语法只涵盖纯文本可以涵盖的范围。

不在 Markdown 涵盖范围之外的标签，都可以直接在文件里面用 HTML 撰写。不需要额外标注这是 HTML 或是 Markdown；只要直接加标签就可以了。

只有区块元素──比如 `<div>`、`<table>`、`<pre>`、`<p>` 等标签，必须在前后加上空行，以利与内容区隔。而且这些（元素）的开始与结尾标签，不可以用 tab 或是空白来缩排。Markdown 的产生器有智能型判断，可以避免在区块标签前后加上没有必要的 `<p>` 标签。

举例来说，在 Markdown 文件里加上一段 HTML 表格：

    This is a regular paragraph.

    <table>
        <tr>
            <td>Foo</td>
        </tr>
    </table>

    This is another regular paragraph.

试试效果：
 <table>
    <tr>
        <td>Foo</td>
    </tr>
</table>

请注意，Markdown 语法在HTML区块中将不会被进行处理。例如，你无法在 HTML 区块内使用 Markdown 形式的`*强调*`。

HTML 的区段标签如 `<span>`、`<cite>`、`<del>` 则不受限制，可以在 Markdown 的段落、清单或是标题里任意使用。依照个人习惯，甚至可以不用Markdown 格式，而采用 HTML 标签来格式化。举例说明：如果比较喜欢 HTML 的  `<a>` 或 `<img>` 标签，可以直接使用这些标签，而不用 Markdown 提供的链接或是影像标示语法。

HTML 区段卷标和区块卷标不同，在区段标签的范围内， Markdown 的语法是有效的。

<h3 id="autoescape">特殊字符自动转换</h3>

在 HTML 文件中，有两个字符需要特殊处理： `<` 和 `&` 。 `<` 符号用于起始卷标，`&` 符号则用于标记 HTML 实体，如果你只是想要使用这些符号，你必须要使用实体的形式，像是 `&lt;` 和 `&amp;`。

`&` 符号其实很容易让写作网络文件的人感到困扰，如果你要打「AT&T」 ，你必须要写成「`AT&amp;T`」 ，还得转换网址内的 `&` 符号，如果你要链接到：

    http://images.google.com/images?num=30&q=larry+bird

你必须要把网址转成：

    http://images.google.com/images?num=30&amp;q=larry+bird

才能放到链接卷标的 `href` 属性里。不用说也知道这很容易忘记，这也可能是 HTML 标准检查所检查到的错误中，数量最多的。

Markdown 允许你直接使用这些符号，但是你要小心转义字符的使用，如果你是在HTML 实体中使用 `&` 符号的话，它不会被转换，而在其它情形下，它则会被转换成 `&amp;`。所以你如果要在文件中插入一个著作权的符号，你可以这样写：

    &copy;

Markdown 将不会对这段文字做修改，但是如果你这样写：

    AT&T

Markdown 就会将它转为：

    AT&amp;T


类似的状况也会发生在 `<` 符号上，因为 Markdown 支持 [行内 HTML](#html) ，如果你是使用 `<` 符号作为 HTML 卷标使用，那 Markdown 也不会对它做任何转换，但是如果你是写：

    4 < 5

Markdown 将会把它转换为：

    4 &lt; 5

不过需要注意的是，code 范围内，不论是行内还是区块， `<` 和 `&` 两个符号都*一定*会被转换成 HTML 实体，这项特性让你可以很容易地用 Markdown 写 HTML code （和 HTML 相对而言， HTML 语法中，你要把所有的 `<` 和 `&` 都转换为 HTML 实体，才能在 HTML 文件里面写出 HTML code。）

* * *

<h2 id="block">区块元素</h2>


<h3 id="p">段落和换行</h3>

一个段落是由一个以上相连接的行句组成，而一个以上的空行则会切分出不同的段落（空行的定义是显示上看起来像是空行，便会被视为空行。比方说，若某一行只包含空白和 tab，则该行也会被视为空行），一般的段落不需要用空白或断行缩排。

「一个以上相连接的行句组成」这句话其实暗示了 Markdown 允许段落内的强迫断行，这个特性和其他大部分的 text-to-HTML 格式不一样（包括 MovableType 的「Convert Line Breaks」选项），其它的格式会把每个断行都转成 `<br />` 标签。

如果你*真的*想要插入 `<br />` 标签的话，在行尾加上两个以上的空白，然后按 enter。

是的，这确实需要花比较多功夫来插入 `<br />` ，但是「每个换行都转换为 `<br />`」的方法在 Markdown 中并不适合， Markdown 中 email 式的 [区块引言][bq] 和多段落的 [清单][l] 在使用换行来排版的时候，不但更好用，还更好阅读。

  [bq]: #blockquote
  [l]:  #list

<h3 id="header">标题</h3>

Markdown 支持两种标题的语法，[Setext][1] 和 [atx][2] 形式。

Setext 形式是用底线的形式，利用 `=` （最高阶标题）和 `-` （第二阶标题），例如：

    This is an H1
    =============

    This is an H2
    -------------

试试效果：

This is an H1
=============
This is an H2
-------------


任何数量的 `=` 和 `-` 都可以有效果。

Atx 形式则是在行首插入 1 到 6 个 `#` ，对应到标题 1 到 6 阶，例如：

    # This is an H1

    ## This is an H2

    ###### This is an H6

试试效果：
# This is an H1

## This is an H2

###### This is an H6


你可以选择性地「关闭」atx 样式的标题，这纯粹只是美观用的，若是觉得这样看起来比较舒适，你就可以在行尾加上 `#`，而行尾的 `#` 数量也不用和开头一样（行首的井字数量决定标题的阶数）：

    # This is an H1 #

    ## This is an H2 ##

    ### This is an H3 ######


<h3 id="blockquote">区块引用</h3>

Markdown 使用 email 形式的区块引言，如果你很熟悉如何在 email 信件中引言，你就知道怎么在 Markdown 文件中建立一个区块引言，那会看起来像是你强迫断行，然后在每行的最前面加上 `>` ：

    > This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
    > consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
    > Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
    > 
    > Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
    > id sem consectetuer libero luctus adipiscing.

Markdown 也允许你只在整个段落的第一行最前面加上 `>` ：

    > This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
    consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
    Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

    > Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
    id sem consectetuer libero luctus adipiscing.

区块引言可以有阶层（例如：引言内的引言），只要根据层数加上不同数量的 `>` ：

    > This is the first level of quoting.
    >
    > > This is nested blockquote.
    >
    > Back to the first level.

引言的区块内也可以使用其他的 Markdown 语法，包括标题、列表、程序代码区块等：

	> ## This is a header.
	> 
	> 1.   This is the first list item.
	> 2.   This is the second list item.
	> 
	> Here's some example code:
	> 
	>     return shell_exec("echo $input | $markdown_script");

试试效果：
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.
> This is the first level of quoting.
>
>> This is nested blockquote.
>
> Back to the first level.
> ## This is a header.
> 
> 1. This is the first list item.
> 2. This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");


任何标准的文本编辑器都能简单地建立 email 样式的引言，例如 BBEdit ，你可以选取文字后然后从选单中选择*增加引言阶层*。

<h3 id="list">清单</h3>

Markdown 支持有序列表和无序列表。

无序清单使用星号、加号或是减号作为列表标记：

    *   Red
    *   Green
    *   Blue

等同于：

    +   Red
    +   Green
    +   Blue

也等同于：

    -   Red
    -   Green
    -   Blue

有序列表则使用数字接着一个英文句点：

    1.  Bird
    2.  McHale
    3.  Parish

很重要的一点是，你在列表标记上使用的数字并不会影响输出的 HTML 结果，上面的列表所产生的 HTML 标记为：

    <ol>
    <li>Bird</li>
    <li>McHale</li>
    <li>Parish</li>
    </ol>


如果你的列表标记写成：

    1.  Bird
    1.  McHale
    1.  Parish

或甚至是：

    3. Bird
    1. McHale
    8. Parish

1.  Bird
1.  McHale
1.  Parish
3. Bird
1. McHale
8. Parish

你都会得到完全相同的 HTML 输出。重点在于，你可以让 Markdown 文件的列表数字和输出的结果相同，或是你懒一点，你可以完全不用在意数字的正确性。

如果你使用懒惰的写法，建议第一个项目最好还是从 1. 开始，因为 Markdown 未来可能会支持有序列表的 start 属性。

列表项目标记通常是放在最左边，但是其实也可以缩排，最多三个空白，项目标记后面则一定要接着至少一个空白或 tab。

要让清单看起来更漂亮，你可以把内容用固定的缩排整理好：

    *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
        Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
        viverra nec, fringilla in, laoreet vitae, risus.
    *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
        Suspendisse id sem consectetuer libero luctus adipiscing.

但是如果你很懒，那也不一定需要：

    *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
    *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.

试试效果：

*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
    Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
    viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
    Suspendisse id sem consectetuer libero luctus adipiscing.
*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
viverra nec, fringilla in, laoreet vitae, risus.
*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
Suspendisse id sem consectetuer libero luctus adipiscing.


如果列表项目间用空行分开， Markdown 会把项目的内容在输出时用 `<p>` 
标签包起来，举例来说：

    *   Bird
    *   Magic

会被转换为：

    <ul>
    <li>Bird</li>
    <li>Magic</li>
    </ul>

但是这个：

    *   Bird

    *   Magic

会被转换为：

    <ul>
    <li><p>Bird</p></li>
    <li><p>Magic</p></li>
    </ul>

列表项目可以包含多个段落，每个项目下的段落都必须缩排 4 个空白或是一个 tab ：

    1.  This is a list item with two paragraphs. Lorem ipsum dolor
        sit amet, consectetuer adipiscing elit. Aliquam hendrerit
        mi posuere lectus.

        Vestibulum enim wisi, viverra nec, fringilla in, laoreet
        vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
        sit amet velit.

    2.  Suspendisse id sem consectetuer libero luctus adipiscing.

如果你每行都有缩排，看起来会看好很多，当然，再次地，如果你很懒惰，Markdown 也允许：

    *   This is a list item with two paragraphs.

        This is the second paragraph in the list item. You're
    only required to indent the first line. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit.

    *   Another item in the same list.

如果要在列表项目内放进引言，那 `>` 就需要缩排：

    *   A list item with a blockquote:

        > This is a blockquote
        > inside a list item.

试试效果:

*   A list item with a blockquote:

    > This is a blockquote
    > inside a list item.


如果要放程序代码区块的话，该区块就需要缩排*两次*，也就是 8 个空白或是两个 tab：

    *   A list item with a code block:

            <code goes here>
试试效果：

*   A list item with a code block:

        <code goes here>


当然，项目列表很可能会不小心产生，像是下面这样的写法：

    1986. What a great season.

换句话说，也就是在行首出现*数字-句点-空白*，要避免这样的状况，你可以在句点前面加上反斜杠。

    1986\. What a great season.


<h3 id="precode">程序代码区块</h3>

和程序相关的写作或是卷标语言原始码通常会有已经排版好的程序代码区块，通常这些区块我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，Markdown 会用 `<pre>` 和 `<code>` 标签来把程序代码区块包起来。

要在 Markdown 中建立程序代码区块很简单，只要简单地缩排 4 个空白或是 1 个 tab 就可以，例如，下面的输入：

    This is a normal paragraph:

        This is a code block.

Markdown 会转换成：

    <p>This is a normal paragraph:</p>

    <pre><code>This is a code block.
    </code></pre>

这个每行一阶的缩排（4 个空白或是 1 个 tab），都会被移除，例如：

    Here is an example of AppleScript:

        tell application "Foo"
            beep
        end tell

会被转换为：

    <p>Here is an example of AppleScript:</p>

    <pre><code>tell application "Foo"
        beep
    end tell
    </code></pre>

一个程序代码区块会一直持续到没有缩排的那一行（或是文件结尾）。

在程序代码区块里面， `&` 、 `<` 和 `>` 会自动转成 HTML 实体，这样的方式让你非常容易使用 Markdown 插入范例用的 HTML 原始码，只需要复制贴上，再加上缩排就可以了，剩下的 Markdown 都会帮你处理，例如：

        <div class="footer">
            &copy; 2004 Foo Corporation
        </div>

会被转换为：

    <pre><code>&lt;div class="footer"&gt;
        &amp;copy; 2004 Foo Corporation
    &lt;/div&gt;
    </code></pre>

程序代码区块中，一般的 Markdown 语法不会被转换，像是星号便只是星号，这表示你可以很容易地以 Markdown 语法撰写 Markdown 语法相关的文件。

<h3 id="hr">分隔线</h3>

你可以在一行中用三个或以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号中间插入空白。下面每种写法都可以建立分隔线：

    * * *

    ***

    *****

    - - -

    ---------------------------------------


* * *

<h2 id="span">区段元素</h2>

<h3 id="link">链接</h3>

Markdown 支持两种形式的链接语法： *行内*和*参考*两种形式。

不管是哪一种，链接的文字都是用 [方括号] 来标记。

要建立一个行内形式的连结，只要在方块括号后面马上接着括号并插入网址连结即可，如果你还想要加上链接的 title 文字，只要在网址后面，用双引号把 title 文字包起来即可，例如：

    This is [an example](http://example.com/ "Title") inline link.

    [This link](http://example.net/) has no title attribute.

会产生：

    <p>This is <a href="http://example.com/" title="Title">
    an example</a> inline link.</p>

    <p><a href="http://example.net/">This link</a> has no
    title attribute.</p>

如果你是要链接到同样主机的资源，你可以使用相对路径：

    See my [About](/about/) page for details.   

参考形式的连结使用另外一个方括号接在链接文字的括号后面，而在第二个方括号里面要填入用以辨识链接的标签：

    This is [an example][id] reference-style link.

你也可以选择性地在两个方括号中间加上空白：

    This is [an example] [id] reference-style link.

接着，在文件的任意处，你可以把这个标签的链接内容定义出来：

    [id]: http://example.com/  "Optional Title Here"

连结定义的形式为：

*   方括号，里面输入链接的辨识用标签
*   接着一个冒号
*   接着一个以上的空白或 tab
*   接着连结的网址
*   选择性地接着 title 内容，可以用单引号、双引号或是括号包着

下面这三种连结的定义都是相同：

	[foo]: http://example.com/  "Optional Title Here"
	[foo]: http://example.com/  'Optional Title Here'
	[foo]: http://example.com/  (Optional Title Here)

**请注意：**有一个已知的问题是 Markdown.pl 1.0.1 会忽略单引号包起来的连结 title。

连结网址也可以用方括号包起来：

    [id]: <http://example.com/>  "Optional Title Here"

你也可以把 title 属性放到下一行，也可以加一些缩排，网址太长的话，这样会比较好看：

    [id]: http://example.com/longish/path/to/resource/here
        "Optional Title Here"

网址定义只有在产生连结的时候用到，并不会直接出现在文件之中。

链接辨识卷标可以有字母、数字、空白和标点符号，但是并*不*区分大小写，因此下面两个连结是一样的：

	[link text][a]
	[link text][A]

*默认的链接卷标*功能让你可以省略指定链接标签，这种情形下，链接卷标和链接文字会视为相同，要用默认链接卷标只要在链接文字后面加上一个空的方括号，如果你要让 "Google" 连结到 google.com，你可以简化成：

	[Google][]

然后定义连结内容：

	[Google]: http://google.com/

由于链接文字可能包含空白，所以这种简化的标签内也可以包含多个文字：

	Visit [Daring Fireball][] for more information.

然后接着定义连结：
	
	[Daring Fireball]: http://daringfireball.net/

连结的定义可以放在文件中的任何一个地方，我比较偏好直接放在连结出现段落的后面，你也可以把它放在文件最后面，就像是批注一样。

下面是一个参考式链接的范例：

    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].

      [1]: http://google.com/        "Google"
      [2]: http://search.yahoo.com/  "Yahoo Search"
      [3]: http://search.msn.com/    "MSN Search"

如果改成用连结名称的方式写：

    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].

      [google]: http://google.com/        "Google"
      [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
      [msn]:    http://search.msn.com/    "MSN Search"

上面两种写法都会产生下面的 HTML。

    <p>I get 10 times more traffic from <a href="http://google.com/"
    title="Google">Google</a> than from
    <a href="http://search.yahoo.com/" title="Yahoo Search">Yahoo</a>
    or <a href="http://search.msn.com/" title="MSN Search">MSN</a>.</p>

下面是用行内形式写的同样一段内容的 Markdown 文件，提供作为比较之用：

    I get 10 times more traffic from [Google](http://google.com/ "Google")
    than from [Yahoo](http://search.yahoo.com/ "Yahoo Search") or
    [MSN](http://search.msn.com/ "MSN Search").

参考式的连结其实重点不在于它比较好写，而是它比较好读，比较一下上面的范例，使用参考式的文章本身只有 81 个字符，但是用行内形式的连结却会增加到 176 个字符，如果是用纯 HTML 格式来写，会有 234 个字符，在 HTML 格式中，卷标比文字还要多。

使用 Markdown 的参考式连结，可以让文件更像是浏览器最后产生的结果，让你可以把一些标记相关的信息移到段落文字之外，你就可以增加连结而不让文章的阅读感觉被打断。

<h3 id="em">强调</h3>

Markdown 使用星号（`*`）和底线（`_`）作为标记强调字词的符号，被 `*` 或 `_` 包围的字词会被转成用 `<em>` 标签包围，用两个 `*` 或 `_` 包起来的话，则会被转成 `<strong>`，例如：

    *single asterisks*

    _single underscores_

    **double asterisks**

    __double underscores__

会转成：

    <em>single asterisks</em>

    <em>single underscores</em>

    <strong>double asterisks</strong>

    <strong>double underscores</strong>

你可以随便用你喜欢的样式，唯一的限制是，你用什么符号开启卷标，就要用什么符号结束。

强调也可以直接插在文字中间：

    un*frigging*believable

但是如果你的 `*` 和 `_` 两边都有空白的话，它们就只会被当成普通的符号。

如果要在文字前后直接插入普通的星号或底线，你可以用反斜杠：

    \*this text is surrounded by literal asterisks\*

<h3 id="code">程序代码</h3>

如果要标记一小段行内程序代码，你可以用反引号把它包起来（`` ` ``），例如：

    Use the `printf()` function.

会产生：

    <p>Use the <code>printf()</code> function.</p>

如果要在程序代码区段内插入反引号，你可以用多个反引号来开启和结束程序代码区段：

    ``There is a literal backtick (`) here.``

这段语法会产生：

    <p><code>There is a literal backtick (`) here.</code></p>

程序代码区段的起始和结束端都可以放入一个空白，起始端后面一个，结束端前面一个，这样你就可以在区段的一开始就插入反引号：

	A single backtick in a code span: `` ` ``
	
	A backtick-delimited string in a code span: `` `foo` ``

会产生：

	<p>A single backtick in a code span: <code>`</code></p>
	
	<p>A backtick-delimited string in a code span: <code>`foo`</code></p>

在程序代码区段内，`&` 和方括号都会被转成 HTML 实体，这样会比较容易插入 HTML 原始码，Markdown 会把下面这段：

    Please don't use any `<blink>` tags.

转为：

    <p>Please don't use any <code>&lt;blink&gt;</code> tags.</p>

你也可以这样写：

    `&#8212;` is the decimal-encoded equivalent of `&mdash;`.

以产生：

    <p><code>&amp;#8212;</code> is the decimal-encoded
    equivalent of <code>&amp;mdash;</code>.</p>



<h3 id="img">图片</h3>

很明显地，要在纯文本应用中设计一个 「自然」的语法来插入图片是有一定难度的。

Markdown 使用一种和链接很相似的语法来标记图片，同样也允许两种样式： *行内*和*参考*。

行内图片的语法看起来像是：

    ![Alt text](/path/to/img.jpg)

    ![Alt text](/path/to/img.jpg "Optional title")

详细叙述如下：

*   一个惊叹号 `!`
*   接着一对方括号，里面放上图片的替换文字
*   接着一对普通括号，里面放上图片的网址，最后还可以用引号包住并加上
    选择性的 'title' 文字。

参考式的图片语法则长得像这样：

    ![Alt text][id]

「id」是图片参考的名称，图片参考的定义方式则和连结参考一样：

    [id]: url/to/image  "Optional title attribute"

到目前为止， Markdown 还没有办法指定图片的宽高，如果你需要的话，你可以使用普通的 `<img>` 标签。

* * *

<h2 id="misc">其它</h2>

<h3 id="autolink">自动链接</h3>

Markdown 支持比较简短的自动链接形式来处理网址和电子邮件信箱，只要是用方括号包起来， Markdown 就会自动把它转成链接，链接的文字就和链接位置一样，例如：

    <http://example.com/>
    
Markdown 会转为：

    <a href="http://example.com/">http://example.com/</a>

自动的邮件连结也很类似，只是 Markdown 会先做一个编码转换的过程，把文字字符转成 16 进位码的 HTML 实体，这样的格式可以混淆一些不好的信箱地址收集机器人，例如：

    <address@example.com>

Markdown 会转成：

    <a href="&#x6D;&#x61;i&#x6C;&#x74;&#x6F;:&#x61;&#x64;&#x64;&#x72;&#x65;
    &#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;
    &#109;">&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;
    &#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;</a>

在浏览器里面，这段字符串会变成一个可以点击的「address@example.com」连结。

（这种作法虽然可以混淆不少的机器人，但并无法全部挡下来，不过这样也比什么都不做好些。无论如何，公开你的信箱终究会引来广告信件的。）

<h3 id="backslash">转义字符</h3>

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果（但不用 `<em>` 标签），你可以在星号的前面加上反斜杠：

    \*literal asterisks\*

Markdown 支持在下面这些符号前面加上反斜杠来帮助插入普通的符号：


    \   反斜杠
    `   反引号
    *   星号
    _   底线
    {}  大括号
    []  方括号
    ()  小括号
    #   井号
	+	加号
	-	减号
    .   英文句号
    !   惊叹号

<h2 id="extra">Markdown扩展</h2>

<h3 id="inline-html">Markdown内嵌HTML</h3>

基本的Markdown语法中，你可以在文档中插入HTML，但其有一个限制，如使用`<div>` `<table>` `<pre>` `<p>`等块级元素里，要使用空行与Markdown文档分隔，开始与结束标签不能由制表符或空格缩进。

Markdown Extra则允许：

1. 块元素的开始标签可以使用不超过三个空格的缩进，超过三个空格将按照基本Markdown语法解释为一个代码块。

2. 当块元素里有列表时，它的每一个列表项可以有相同的缩进。（缩进更多也可以，只要块元素的开始标签不要缩进太多，使它们被解释成一个代码块。）

<h2 id="markdown-in-html">HTML内嵌Markdown</h2>

Markdown Extra可以在HTML中内嵌Markdown句，基本Markdown语法中，HTML标签之内的内容并不能使用Markdown语法，但在PHP Markdown Extra中，给HTML标签加上一个值为1的markdown属性后，就可以在任何块级标签中使用Markdown语法了，例如：

    <div markdown="1"> This is *true* markdown text. </div>

生成：

    <div> <p>This is <em>true</em> markdown text.</p> </div>

 试试效果：
<div markdown="1"> This is *true* markdown text. </div>

------

PHP Markdown Extra的内嵌Markdown只会应用于块级元素的下一级元素，如果你把Markdown属性放到一个`<p>`标签里，Markdown语法只会在`<span>`级的元素里起作用，并不会在列表，引用文字，代码块上起作用。

但下面这种情况是模棱两可的：

    <table> <tr> <td markdown="1">This is *true* markdown text.</td> </tr> </table> 


表格的单元格既可以包含内联元素，也可以包含块元素，要让Markdown语法作用于内联元素与块元素，可以使用属性`markdown="block"`。

<h3 id="special-attribute">特殊属性</h3>

使用PHP Markdown Extra你可以为块元素设置id和class属性。看例子：


    Header 1    {#header1}
    ========
    ## Header 2 {.header2}


生成：


	<h1 id="header1">Header 1</h1>
	<h2 class="header2">Header 2</h2>


又如：

	## The Site {.main .shine #the-site}

生成：

	<h2 class="main shine" id="the-site">The Site</h2>

特殊属性可以应用于：

* 标题
* 代码块
* 链接
* 图片


<h2 id="fenced-code-blocks">代码块</h2>

相比于基本的Markdown语法，增加了用三个或三个以上~来标记代码块。如：

	This is a normal paragraph
    
	~~~~~~~~~~~~~~~~~~~~~
	This is a code block
	~~~~~~~~~~~~~~~~~~~~~

生成：

    <p>This is a normal paragraph:</p>
    
    <pre><code>This is a code block.
    </code></pre>

`~`标记的代码块的上下可以包含空行


    ~~~
    
    blank line before
    blank line after

    ~~~


在基本的Markdown语法中，代码块并不能用到一个列表项里，因为在列表项里一个缩进代表的是另一个段落的开始，但是在PHP Markdown Extra中，用~可以做到把代码块放到列表项中，如：

    1.  List item
    
        Not an indented code block, but a second paragraph
        in the list item

        ~~~~
        This is a code block, fenced-style
        ~~~~


还可以为代码块标明一个class name，如：

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~.html
    <p>paragraph <b>emphasis</b>
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~

也可以使用特殊属性来标记：

    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~{.html #example-1}
    <p>paragraph <b>emphasis</b>
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~


<h2 id="tables">表格</h2>

PHP Markdown Extra可以制作简单的表格，如：


    First Header  | Second Header
    ------------- | -------------
    Content Cell  | Content Cell
    Content Cell  | Content Cell

生成：

    <table>
    <thead>
    <tr>
      <th>First Header</th>
      <th>Second Header</th>
    </tr>
    </thead>
    <tbody>
    <tr>
      <td>Content Cell</td>
      <td>Content Cell</td>
    </tr>
    <tr>
      <td>Content Cell</td>
      <td>Content Cell</td>
    </tr>
    </tbody>
    </table>

如果你愿意，也可以写成：

    | First Header  | Second Header |
    | ------------- | ------------- |
    | Content Cell  | Content Cell  |
    | Content Cell  | Content Cell  |

生成的代码和上面是一样的。
试试效果：

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |

还可以实现左右对齐，居中对齐的表格。就是在表头分隔线的左右两边放置:，:放在左边为左对齐，放在右边为右对齐，两边者放置:为居中对齐，以下为居中对齐：

    | Item        | Value   |
    | :---------: | :-----: |
    | Computer    | $1600   |
    | Phone       | $12     |
    | Pipe        | $1      |

试试效果：

| Item        | Value   |
| :---------: | :-----: |
| Computer    | $1600   |
| Phone       | $12     |
| Pipe        | $1      |

译注：此处生成的代码是在th与td标签内添加align属性，此属性在HTML5中已被弃用，改用CSS来实现。

你也可以在表格中使用内联级的Markdwon语法，如：

    | Function name | Description                    |
    | ------------- | ------------------------------ |
    | `help()`      | Display the help window.       |
    | `destroy()`   | **Destroy your computer!**     |

试试效果

| Function name | Description                    |
| ------------- | ------------------------------ |
| `help()`      | Display the help window.       |
| `destroy()`   | **Destroy your computer!**     |

<h3 id="defination-lists">定义列表</h3>

PHP Markdown Extra 也可以生成定义列表，定义描述以一个冒号开始，两个定义项之间要用一个以上空行分开。如：

    Apple
    :   Pomaceous fruit of plants of the genus Malus in
        the family Rosaceae.
    
    Orange
    :   The fruit of an evergreen tree of the genus Citrus.
    
或者你也可以偷懒，写成：

    Apple
    :   Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.

    Orange
    :   The fruit of an evergreen tree of the genus Citrus.
   
将生成如下HTML：

    <dl>
    <dt>Apple</dt>
    <dd>Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.</dd>
    
    <dt>Orange</dt>
    <dd>The fruit of an evergreen tree of the genus Citrus.</dd>
    </dl>

冒号通常在最左边输入，但是你也可以以少于等于三个的空格进行缩进。冒号后面应该有一个空格以上的分隔

一个定义项可以有多个定义描述：

    Apple
    :   Pomaceous fruit of plants of the genus Malus in 
        the family Rosaceae.
    :   An American computer company.
    
    Orange
    :   The fruit of an evergreen tree of the genus Citrus.

一个定义描述也可以有多个定义项：

    Term 1
    Term 2
    :   Definition a

    Term 3
    :   Definition b
 

如果定义描述与定义项用一个空行分开，那么定义描述将用一个<p>标签包含：

    Apple
    
    :   Pomaceous fruit of plants of the genus Malus in 
        the family Rosaceae.
    
    Orange
    
    :    The fruit of an evergreen tree of the genus Citrus.

将生成：

    <dl>
    <dt>Apple</dt>
    <dd>
    <p>Pomaceous fruit of plants of the genus Malus in 
    the family Rosaceae.</p>
    </dd>
    
    <dt>Orange</dt>
    <dd>
    <p>The fruit of an evergreen tree of the genus Citrus.</p>
    </dd>
    </dl>

像正常的定义列表一样，定义描述可以包含多个段落，还有其它的一些块级元素，如引用，代码块，列表，甚至是其它的定义列表。

    Term 1
    
    :   This is a definition with two paragraphs. Lorem ipsum 
        dolor sit amet, consectetuer adipiscing elit. Aliquam 
        hendrerit mi posuere lectus.
    
        Vestibulum enim wisi, viverra nec, fringilla in, laoreet
        vitae, risus.
    
    :   Second definition for term 1, also wrapped in a paragraph
        because of the blank line preceding it.
    
    Term 2
    
    :   This definition has a code block, a blockquote and a list.
    
            code block.
    
        > block quote
        > on two lines.
    
        1.  first list item
        2.  second list item

试试效果：

Term 1

:   This is a definition with two paragraphs. Lorem ipsum 
    dolor sit amet, consectetuer adipiscing elit. Aliquam 
    hendrerit mi posuere lectus.

    Vestibulum enim wisi, viverra nec, fringilla in, laoreet
    vitae, risus.

:   Second definition for term 1, also wrapped in a paragraph
    because of the blank line preceding it.

Term 2

:   This definition has a code block, a blockquote and a list.

        code block.

    > block quote
    > on two lines.

    1.  first list item
    2.  second list item

<h2 id="footnotes">脚注</h2>

在PHP Markdown Extra 里，脚注的写法与链接的写法差不多。在文章里，脚注会变成上标的形式，脚注的内容你可以在文档的末尾来添加，就如下面这样：

    That's some text with a footnote.[^1]
    
    [^1]: And that's the footnote.

将生成如下HTML：

    <p>That's some text with a footnote.
       <sup id="fnref-1"><a href="#fn-1" class="footnote-ref">1</a></sup></p>
    
    <div class="footnotes">
    <hr />
    <ol>
    
    <li id="fn-1">
    <p>And that's the footnote.
       <a href="#fnref-1" class="footnote-backref">&#8617;</a></p>
    </li>
    
    </ol>
    </div>

试试效果：

That's some text with a footnote.[^1]

[^1]: And that's the footnote.

在文档的很多地方你能发现脚注，但是在同一篇文档里，你不能有两个相同的脚注，如果你使用了两个相同的脚注，则第二个脚注将被当作普通的文本来对待。

每个脚注应该有不同的名字，名字可以为除了HTML里的缩略短语外的任何字符。

脚注内容可以包含块级元素，如多个段落，列表，引用等等，它的用法和定义列表相似，如下面的第二个段落以四个空格为缩进：

    That's some text with a footnote.[^1]
    
    [^1]: And that's the footnote.
    
    That's the second paragraph.
    
 
如果你想对齐，你也可以像下面这样：

     [^1]:
        And that's the footnote.
    
        That's the second paragraph.

<h2 id="abbreviations">缩略短语</h2>

PHP Markdown Extra 支持缩略短语 (HTML tag )，你可以像下面这样创建一个缩略短语：

    *[HTML]: Hyper Text Markup Language
    *[W3C]:  World Wide Web Consortium
   
然后在正文里这样写：

    The HTML specification
    is maintained by the W3C.
 

将会生成：

    The <abbr title="Hyper Text Markup Language">HTML</abbr> specification
    is maintained by the <abbr title="World Wide Web Consortium">W3C</abbr>.

试试效果：

*[HTML]: Hyper Text Markup Language
*[W3C]:  World Wide Web Consortium

The HTML specification is maintained by the W3C. 
    
一个缩略短语可以包含多个单词，而且大小写敏感，缩略短语可以为空，此时生成的<abbr>标签的title就为空，如：

    Operation Tigra Genesis is going well.
    
    *[Tigra Genesis]:

<h2 id="extra-emphasis">强调</h2>

PHP Markdown Extra对常规的Markdown语法做了一点点改变，单词中间的下划线并不会被解释为HTML强调标签，如：

    Please open the folder "secret_magic_box".

PHP Markdown Extra 将会解释成：

    <p>Please open the folder "secret_magic_box".</p>


但是如果你想强调一个单词的一部分，你可以用星号来代替。
像下面这样，下划线是能被正确解释的（也即可以多个词同时强调）：

    I like it when you say _you love me_.
    
<h2 id="backslash-escapes">新增的转义字符</h2>

综合上面全文可以明显的看出，在基本Markdown语法上，新增加`:` 和 `|` 要用反斜杠`\`来转义。


<h1 id="reference">参考资料</h1>

1. https://en.wikipedia.org/wiki/Markdown
2. http://markdown.tw
3. http://daringfireball.net/projects/markdown/syntax
4. http://b.soont.com/md/php-markdown
5. https://michelf.ca/projects/php-markdown/extra
5. https://github.com/RT-Thread/rtthread-manual-doc/blob/master/zh/2appendix/02-chapter_markdown.md