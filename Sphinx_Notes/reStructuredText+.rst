.. _reStructuredText+:

############################
reStructuredText+
############################

.. note::

    并非只是reStructuredText，这一章还包含了一些常用Sphinx的特色，比如指令和角色。

===========================
Paragraphs
===========================

The paragraph is the most basic block
in a reStructuredText document.
Paragraphs are simply chunks of text separated by one or more blank lines.
As in Python, indentation is significant in reStructuredText,
so all lines of the same paragraph must be left-aligned
to the same level of indentation.

===========================
Inline markup
===========================

* one asterisk: ``*text*`` for emphasis (italics),
    实际上是 `emphasis` 角色 ``:emphasis:`text```
* two asterisks: ``**text**`` for strong emphasis (boldface),
    实际上是 `strong` 角色 ``:strong:`text```
* backquotes: ````text```` for code samples (literal),
    实际上是 `literal` 角色 ``:literal:`text```
* backquote: ```text``` for title references (italics),
    实际上是 ``title-reference`` 角色，别名 ``title`` , ``:title:`text``

    也是解释性文本角色，可用 ``.. default-role::`` 指令来更改其含义。

效果:

    *text*
    **text**
    ``text``
    `text`

如果文本中的星号或者反单引号可能被混淆为行内标记分隔符，添加反斜杠转义即可。
注意，这些标记有些功能限制：

* 不能嵌套，
* 标记内不能以空格开始： ``* 文本*`` 就不行，
* 行内标记符号外只能是非文字字符。想写空格的话就要转义： ``thisis\ *one*\ word``

===========================
Lists
===========================

有三种风格来表示一列项目。无序列表用 `*`, `-`, `+` 做项目符号，
有序列表可以用 数字、字母、罗马数字 加上 点（`.`）、右英文括号（`)`）或用英文括号完全包围 -- 无论你偏好什么，都能识别::

    *   无序 1

    -   无序 2

    +   无序 3

    1.  有序 1

    2)  有序 2)

    (3) 有序 (3)

    i.  有序 一

    II.  有序 贰

    c.  有序 three

效果：

*   无序 1

-   无序 2

+   无序 3

1.  有序 1

2)  有序 2)

(3) 有序 (3)

i.  有序 一

II.  有序 贰

c.  有序 three

.. tip::

    无序列表的项目符号可以混用，只需要保持缩进即可。

    但有序列表的项目符号不可混用，并且需要保持编号连续且单调。
    不过，你可以使用 `#` 符号来进行编号的自动推导::

        1. 第一项
        #. 第二
        #. 第三

    1. 第一项
    #. 第二
    #. 第三

也可以嵌套列表，但注意它们必须通过空行与父列表项分开::

    *   this is
    *   a list

        1. with a nested list
        #. and some subitems

    *   and here the parent list continues

定义列表创建如下::

   term (up to a line of text)
      Definition of the term, which must be indented

      and can even consist of multiple paragraphs

   next term
      Description.

效果:

 term (up to a line of text)
    Definition of the term, which must be indente
    and can even consist of multiple paragraphs
 next term
    Description.

 Note that the term cannot have more than one line of text.

请注意，一个术语可以有很多段，段与段之间用空行分隔，但一段只能有一行文本。

引用的段落只是通过缩进它们来创建，而不是根据周围的段落创建。

行块是一种保留换行符的方法::

   | These lines are
   | broken exactly like in
   | the source file.

效果:

| These lines are
| broken exactly like in
| the source file.


.. _rst-literal-blocks:

===============================
Literal blocks
===============================

文字代码块是通过在段落结束时使用特殊标记“::”来引入的。
文字块必须缩进（和所有段落一样，用空行隔开周围的段落）::

   This is a normal text paragraph. The next paragraph is a code sample::

      It is not processed in any way, except
      that the indentation is removed.

      It can span multiple lines.

   This is a normal text paragraph again.

效果:

This is a normal text paragraph. The next paragraph is a code sample::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.

“::”标记的处理很灵活：

* 如果它作为一个单独的段落出现，那么该段落将完全从文档中删除。
* 如果前面有空格，则删除该标记。
* 如果前面有非空格，则该标记将被单个冒号替换。

.. note::

    关于文字块中的代码高亮，见 :ref:`highlight <rst-showing-code>`

.. _rst-doctest-blocks:

==============================
Doctest blocks
==============================

Doctest块是将交互式的Python会话剪切粘贴到文档字符串中。它们不需要
:ref:`literal blocks <rst-literal-blocks>` 语法。
doctest 块必须以空行结束，并且 *不* 以未使用的提示符结束::

    >>> 1 + 1
    2

效果:

>>> 1 + 1
2


==============================
Tables
==============================

对于网格式表格，你必须自己“绘制”单元格网格。它们是这样的::

   +------------------------+------------+----------+----------+
   | Header row, column 1   | Header 2   | Header 3 | Header 4 |
   | (header rows optional) |            |          |          |
   +========================+============+==========+==========+
   | body row 1, column 1   | column 2   | column 3 | column 4 |
   +------------------------+------------+----------+----------+
   | body row 2             | ...        | ...      |          |
   +------------------------+------------+----------+----------+

效果:

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+

简单表格更容易撰写，但有限制：它们必须包含不止一行，第一列单元格不能包含多行。如::

   =====  =====  =======
   A      B      A and B
   =====  =====  =======
   False  False  False
   True   False  False
   False  True   False
   True   True   True
   =====  =====  =======

效果:

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

除了网格式和简单式的表格之外，还可以使用 `list-table <https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table>`_
或 `csv-table <https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table>`_ 
来创建表格， 和前两种相比，后两种比较不美观，但是不需要做 “字符画” 了。

**CSV Table 例子**::

    .. csv-table:: Frozen Delights!
    :header: "Treat", "Quantity", "Description"
    :widths: 15, 10, 30

    "Albatross", 2.99, "On a stick!"
    "Crunchy Frog", 1.49, "If we took the bones out,
    it wouldn't be crunchy, now would it?"
    "Gannet Ripple", 1.99, "On a stick!"

以下选项可被识别：

widths
    auto 或一组整数，设置列宽。默认每列一致。

width
    整体的宽度。

header-rows
    整数，表示接下来的 CSV 数据中前几行为表头。默认 0.

header
    一列 CSV 内容，用作表头。将插入到 header-rows 所设定的行前面。

stub-columns
    整数，表示 CSV 数据中左几列为存根。默认 0.

file
    从文件系统读取 CSV 数据。

url
    从网络地址读取 CSV 数据。

encoding
    设置外部 CSV 数据的字符编码。默认和当前文档相同。

delim
    分隔符，默认逗号 ``,`` 。

quote
    括号，用来包括表格中的单元。默认双引号 ``"`` 。

keepspace
    保留分隔符旁的空白。默认忽略。

escape
    转义符号。默认是将需要转义的字符重复两遍::

    "He said, ""Hi!"", and go away."

align
    水平对齐方式。

**List Table 例子**::

    .. list-table::

        *   -   表头1
            -   表头2
            -   表头3
        *   -   内容11
            -   内容12
            -   内容13
        *   -   内容21
            -   内容22
            -   内容23

用列表的形式来创建表格。 列表的顶级项表示一行，次级项表示一行的各元素。

效果：

.. list-table::

    *   -   表头1
        -   表头2
        -   表头3
    *   -   内容11
        -   内容12
        -   内容13
    *   -   内容21
        -   内容22
        -   内容23

==============================
Hyperlinks
==============================

一个超链接需要有两个部分：引用和靶标::

    引用部分需要在名称后加下划线：链接_
    如果名称中包含了空格，则需要用反引号包括起来：`链 接`_。

    靶标部分的下划线在名称前面：

    .. _链接: https://docutils.sourceforge.io/docs/user/rst/quickref.html

    如果留空，则会将靶标引至下一个块元素。


-------------------------------
External links
-------------------------------

使用 ```Link text <https://domain.invalid/>`_`` 进行行内网络链接，这是比引用和标靶写在同一位置的写法。

效果:
`Link text <https://domain.invalid/>`_

.. important:: 链接文本与 URL 前面的 < 之间必须有空格。

也可以把引用和标靶分开，就像这样::

   This is a paragraph that contains `a link`_.

   .. _a link: https://domain.invalid/

效果:
This is a paragraph that contains `a link`_.

.. _a link: https://domain.invalid/

如果链接文本应该是Web地址，则根本不需要特殊标记，解析器会在普通文本中查找链接和邮件地址，任何满足 Uri 形式的文本会在渲染流程的最后被识别为超链接::

    -   https://docutils.sourceforge.io/docs/user/rst/quickref.html#hyperlink-targets
    -   ftp://firefox.fake-mozilla.org/

效果：

-   https://docutils.sourceforge.io/docs/user/rst/quickref.html#hyperlink-targets
-   ftp://firefox.fake-mozilla.org/


-------------------------------
Internal links
-------------------------------

靶标也有内联形式，例如::

    _`靶标` 在这里，而引用将会引至前面的 靶标_ 处。

_`靶标` 在这里，而引用将会引至前面的 靶标_ 处。

隐式超链接可以将引用引至标题::

    正如下面的 `标题`_ 章节所说一样。

正如下面的 `Sections`_ 章节所说一样。

Internal linking is also done via a special reStructuredText role provided by Sphinx,
see the section on specific markup,
`交叉引用任意位置 <https://www.sphinx-doc.org/zh-cn/master/usage/referencing.html#ref-role>`_ .


============================
Sections
============================

标题是划分章节的依据。将单行文本缀以下划符号则构成标题。
可用的符号有 :literal:`#=-~:'"^_*+<>`，以及反引号。
需要满足长度条件：下划符号的数目与标题文本一致，（中文这类宽字符算两个字符）。

章节的大小关系与符号无关，只与符号出现的顺序有关。一般来讲，习惯用 ``#`` 做一级标题，``=``, ``-`` 分别做 二、三 级标题。

并且，可以使用双划线::

    ##########
    双划线风格
    ##########

    单划线风格
    ==========

标题本身会提供一个锚点，可以使用 `Internal links`_ 的方式来指向本文的一个章节，就是上面 `Internal links`_ 章节说的。

另外，任何四个以上的重复横线将会渲染为分割线::

    ----

效果：

----

=============================
Contents
=============================

``contents`` 指令可以自动生成目录，可以在任何地方使用，但是一般放在文档的开头::

    .. contents::

可接受以下选项：

depth
    整数，设置目录层级深度，默认无限。

local
    如果提供，则会生成该章节以及子章节的目录而非全篇目录。

backlinks
    是否生成目录项和文档项之间的链接。

=============================
Field Lists
=============================

字段列表是这样标记的字段序列::

    :fieldname: Field content

效果:

:fieldname: Field content

它们通常在Python文档中使用::

    def my_function(my_arg, my_other_arg):
        """A function just for me.

        :param my_arg: The first of my arguments.
        :param my_other_arg: The second of my arguments.

        :returns: A message (just for me, of course).
        
        """

效果:

def my_function(my_arg, my_other_arg):
   """A function just for me.

   :param my_arg: The first of my arguments.
   :param my_other_arg: The second of my arguments.

   :returns: A message (just for me, of course).
   
   """

Sphinx extends standard docutils behavior and intercepts field lists specified
at the beginning of documents.  Refer to `field-lists <https://www.sphinx-doc.org/en/master/usage/restructuredtext/field-lists.html>`_ 
for more information.


.. _rst-pictures:

=============================
Pictures
=============================

插入图像可以使用 `image` 或 `figure` 指令。

image 属于直接插入图片用的，而 figure 则可以添加更详细的描述。

Sphinx会自动将图像文件复制到构建的输出目录的子目录中
(例如，用于HTML输出的 ``_static`` 目录。)

image 接受一个参数：图像的 Uri，如果是相对路径，则起点是当前文档，绝对路径的根为source。
image 可接受零个或多个选项，可选的选项有：

height
    图像高度，单位见下方表格。

width
    图像宽度，同上。

scale
    图像缩放，使用百分比。

align
    可以是以下值之一：*top*, *middle*, *bottom*, *left*, *center*, *right*，设置图像对齐方式。

target
    如果设置，需要传入一个超链接靶标。这会让图片可点击，点击后跳转到靶标。
    对于 HTML，是将 img 元素放在了 a 元素内部。

::

    .. image:: img/test.jpg
        :height: 400px
        :width: 600px
        :scale: 50%
        :align: center
        :target: https://docutils.sourceforge.io/docs/ref/rst/directives.html#image

.. image:: img/test.jpg
    :height: 400px
    :width: 600px
    :scale: 50%
    :align: center
    :target: https://docutils.sourceforge.io/docs/ref/rst/directives.html#image

----------------

+-------+-------------------------------------------------------------+
| Unit  | Description                                                 |
+=======+=============================================================+
| em    | the element's font size                                     |
+-------+-------------------------------------------------------------+
| ex    | x-height of the element's font                              |
+-------+-------------------------------------------------------------+
| ch    | width of the “0” (ZERO, U+0030) glyph in the element’s font |
+-------+-------------------------------------------------------------+
| rem   | font size of the root element                               |
+-------+-------------------------------------------------------------+
| vw    | 1% of the viewport (or paper) width                         |
+-------+-------------------------------------------------------------+
| vh    | 1% of the viewport (or paper) height                        |
+-------+-------------------------------------------------------------+
| vmin  | 1% of the viewport’s smaller dimension                      |
+-------+-------------------------------------------------------------+
| vmax  | 1% of the viewport’s larger dimension                       |
+-------+-------------------------------------------------------------+
| cm    | centimeters                                                 |
|       | 1 cm = 10 mm                                                |
+-------+-------------------------------------------------------------+
| mm    | millimeters          |                                      |
|       | 1 mm = 1/1000 m                                             |
+-------+-------------------------------------------------------------+
| Q     | quarter-millimeters  |                                      |
|       | 1 Q = 1/4 mm                                                |
+-------+-------------------------------------------------------------+
| in    | inches               |                                      |
|       | 1 in = 25.4 mm = 96 px                                      |
+-------+-------------------------------------------------------------+
| pc    | picas                |                                      |
|       | 1 pc = 1/6 in = 12 pt                                       |
+-------+-------------------------------------------------------------+
| pt    | points               |                                      |
|       | 1 pt = 1/72 in                                              |
+-------+-------------------------------------------------------------+
| px    | pixels               |                                      |
|       | 1 px = 3/4 pt = 1/96 in                                     |
+-------+-------------------------------------------------------------+

----------------

figure 由 image 和一段标题（一个单行段落），以及可选的图例组成。
对于基于页的媒体（如PDF），在排版时，figure 可能会浮动到合适的地方。

figure 拥有 image 所有的选项，在以下几处有所不同：

align
    可传入 *left*, *center*, *right*。
    只能设置水平方向上的对齐方式。

figwidth
    设置图像宽度，这将影响图像标题和图例的折行方式，以确保它们的宽度不会超过这个值。
    但是这并不影响内嵌的图片宽度，图片的宽度需要用 width 选项设置::

        +---------------------------+
        |        figure             |
        |                           |
        |<------ figwidth --------->|
        |                           |
        |  +---------------------+  |
        |  |     image           |  |
        |  |                     |  |
        |  |<--- width --------->|  |
        |  +---------------------+  |
        |                           |
        |The figure's caption should|
        |wrap at this width.        |
        +---------------------------+

::

    .. figure:: img/test.jpg
        :height: 400px
        :width: 600px
        :scale: 50%
        :align: center
        :target: https://docutils.sourceforge.io/docs/ref/rst/directives.html#image

        けだまつり ｜ 玉之けだま

.. figure:: img/test.jpg
    :height: 400px
    :width: 600px
    :scale: 50%
    :align: center
    :target: https://docutils.sourceforge.io/docs/ref/rst/directives.html#image

    けだまつり ｜ 玉之けだま

=============================
Footnotes
=============================

对于脚注（ref），使用 ``[#name]_`` 标记脚注位置，并在“脚注”标题后添加脚注主体在文档底部，像这样::

   Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

   .. rubric:: Footnotes

   .. [#f1] Text of the first footnote.
   .. [#f2] Text of the second footnote.

您还可以明确编号脚注 (``[1]_``) 或使用不带名字的自动编号脚注 (``[#]_``)。

效果：

Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

.. rubric:: Footnotes

.. [#f1] Text of the first footnote.
.. [#f2] Text of the second footnote.


==============================
Citations
==============================

Standard reStructuredText citations are supported,
with the additional feature that they are "global",
i.e. all citations can be referenced from all files.  Use them like so::

   Lorem ipsum [Ref]_ dolor sit amet.

   .. [Ref] Book or article reference, URL or whatever.

引用用法类似于脚注用法，但标签不是数字或以 ``＃`` 开头。

效果：

Lorem ipsum [Ref]_ dolor sit amet.

.. [Ref] Book or article reference, URL or whatever.


.. _rst-admonitions:

================================
Admonitions
================================

reStructuredText 提供了一些段落级的标记指令，如：

.. hlist::
    :columns: 4

    - attention
    - caution
    - danger
    - error
    - hint
    - important
    - note
    - tip
    - warning
    - see also

用来将传入的体元素表达为指定的语义::

    .. warning::

        我警告你哦！╰（‵□′）╯

效果：

.. warning::

    我警告你哦！╰（‵□′）╯

比较通用的是 ``admonition`` 指令，上述指令可以看作是它的子类::

    .. admonition:: 要闻
        :class: attention

        其实没有什么要闻。

效果：

.. admonition:: 要闻
    :class: attention
    
    其实没有什么要闻。

``:class:`` 其实可以不要：

.. admonition:: 要闻

    其实没有什么要闻。


================================
Comments
================================

单行注释::

   .. This is a comment.

多行注释::

   ..
      This whole indented block
      is a comment.

      Still in the comment.

.. _rst-html-meta:

==================================
HTML Metadata
==================================

The meta directive allows specifying the HTML
`metadata element`_ of a Sphinx documentation page.  For example, the
directive::

   .. meta::
      :description: The Sphinx documentation builder
      :keywords: Sphinx, documentation, builder

will generate the following HTML output:

.. code-block:: html

   <meta name="description" content="The Sphinx documentation builder">
   <meta name="keywords" content="Sphinx, documentation, builder">

此外，Sphinx将按照元指令中指定的关键字添加到搜索索引中。因此，元元素的 ``lang`` 属性被考虑在内。例如，指令::

   .. meta::
      :keywords: backup
      :keywords lang=en: pleasefindthiskey pleasefindthiskeytoo
      :keywords lang=de: bittediesenkeyfinden

adds the following words to the search indices of builds with different language
configurations:

* ``pleasefindthiskey``, ``pleasefindthiskeytoo`` to *English* builds;
* ``bittediesenkeyfinden`` to *German* builds;
* ``backup`` to builds in all languages.

.. _metadata element: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
