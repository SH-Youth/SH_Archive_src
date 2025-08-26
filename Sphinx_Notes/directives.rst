.. highlight:: rst

###########################
Directives
###########################

.. image:: https://moe-counter.sai-hentai.dpdns.org/blog-sphinxnote-directives/
   :alt: 访问量统计

A directive is a generic block of explicit markup.
Along with roles, it is one of the extension mechanisms of reStructuredText,
and Sphinx makes heavy use of it.

基本用法::
   
    .. directive_type:: directive
        :option: value

        content

Docutils supports the following directives:

* Admonitions: ``attention``, ``caution,`` ``danger``, ``error``, ``hint``,
  ``important``, ``note``, ``tip``, ``warning``.  (see also :ref:`rst-admonitions`)

* Images:

  - ``image`` (see also :ref:`rst-pictures`)
  - ``figure`` (see also :ref:`rst-pictures`)

* Additional body elements:

  - ``contents`` (本地，即仅当前文件，目录)
  - ``container`` (一个带有自定义类的容器，用于在HTML中生成外部 ``<div>`` )
  - ``rubric`` (与文档切片无关的标题)
  - ``topic``, ``sidebar`` (突出显示的构成元素)
  - ``parsed-literal`` (支持行内标记的文字块)
  - ``epigraph`` (带有可选归因线的块引用)
  - ``highlights``, ``pull-quote`` (带有自己的类属性的块引号)
  - ``compound`` (复合段)

* Special tables:

  - ``table`` (带标题的表格)
  - ``csv-table`` (从逗号分隔值生成的表)
  - ``list-table`` (从列表列表生成的表)

* Special directives:

  - ``raw`` (include raw target-format markup)
  - ``include`` (include reStructuredText from another file) -- in Sphinx,
    when given an absolute include file path, this directive takes it as
    relative to the source directory
  - ``class`` (assign a class attribute to the next element)

    .. note::

       When the default domain contains a ``class`` directive, this directive
       will be shadowed.  Therefore, Sphinx re-exports it as ``rst-class``.

    .. tip::

       If you want to add a class to a directive,
       you may consider the ``:class:`` `common option <https://docutils.sourceforge.io/docs/ref/rst/directives.html#common-options>`_ 
       instead, which is supported by most directives and allows for a more compact notation.

* HTML specifics:

  - ``meta``
    (generation of HTML ``<meta>`` tags, see also :ref:`rst-html-meta` below)
  - ``title`` (override document title)

* Influncing markup:

  - ``default-role`` (set the default interpreted text role)
  - ``role`` (define a new interpreted text role)

基本上，指令由名称、参数、选项和内容组成。看看这个例子::

   .. function:: foo(x)
                 foo(y, z)
      :module: some.module.name

      Return a line of text input from the user.

``function`` 是指令名称。这里给出了两个参数，第一行的其余部分和第二行，以及一个选项 ``模块``
（如你所见，选项在紧跟在参数后面的行中给出并由冒号表示）。选项必须缩进到与指令内容相同的级别。

The directive content follows after a blank line and is indented relative to
the directive start or if options are present, by the same amount as the
options.

Be careful as the indent is not a fixed number of whitespace, e.g. three, but
any number whitespace.  This can be surprising when a fixed indent is used
throughout the document and can make a difference for directives which are
sensitive to whitespace. Compare::

    .. code-block::
        :caption: A cool example

            The output of this line starts with four spaces.

    .. code-block::

        The output of this line has no spaces at the beginning.

效果：

.. code-block::
    :caption: A cool example

        The output of this line starts with four spaces.

.. code-block::

    The output of this line has no spaces at the beginning.

In the first code block, the indent for the content was fixated by the option
line to three spaces, consequently the content starts with four spaces.
In the latter the indent was fixed by the content itself to seven spaces, thus
it does not start with a space.



====================
Versions and Changes
====================

``.. versionadded::`` version [brief explanation]
    标记自某个特定版本开始添加的功能。
    例如::

        .. versionadded:: 1.0
            Added the `feature` directive.

    效果：

    .. versionadded:: 1.0
        Added the `feature` directive.

``.. versionchanged::`` version [brief explanation]
    标记自某个特定版本开始更改的功能。
    例如::

        .. versionchanged:: 1.0
            The `feature` directive now supports the `option` argument.
    
    效果：

    .. versionchanged:: 1.0
        The `feature` directive now supports the `option` argument.

``.. deprecated::`` version [brief explanation]
    标记自某个特定版本开始弃用的功能。
    例如::

        .. deprecated:: 1.0
            The `feature` directive is deprecated.

    效果：

    .. deprecated:: 1.0
        The `feature` directive is deprecated.

``.. versionremoved::`` version [brief explanation]
    标记自某个特定版本开始移除的功能。这项指令在version 7.3中被加入，目前此主题无法渲染。
    例如::

        .. versionremoved:: 1.0
            The `feature` directive has been removed.


====================
Presentational
====================

``.. rubric::`` 指令用于在文档中添加一个类似标题（heading）的rubric，不会影响文档的结构。
    .. rubric:: Options

    ``:class:`` *class names (a list of class names, separated by spaces)*
        Assign class attributes. This is a common option.

    ``:name:`` *label (text)*
        An implicit target name that can be referenced using ref. This is a common option.

    ``:heading-level:`` *n (number from 1 to 6)*
        Added in version 7.4.1.

        Use this option to specify the heading level of the rubric.
        In this case the rubric will be rendered as <h1> to <h6> for HTML output,
        or as the corresponding non-numbered sectioning command for LaTeX.

--------------------

``.. hlist::`` 指令用于一个包含水平维度的列表
    .. rubric:: Options
    
    ``:columns:`` *n (int)*
        The number of columns; defaults to 2. For example::
            
            .. hlist::
                :columns: 3

                * A list of
                * short items
                * that should be
                * displayed
                * horizontally
        
        效果：

        .. hlist::
            :columns: 3

            * A list of
            * short items
            * that should be
            * displayed
            * horizontally

.. _rst-showing-code:

====================
Showing Code
====================

There are multiple ways to show syntax-highlighted literal code blocks in Sphinx:

- using :ref:`reStructuredText doctest blocks <rst-doctest-blocks>`;

- using :ref:`reStructuredText literal blocks <rst-literal-blocks>`,
  optionally in combination with the ``highlight`` directive;

- using the ``code-block`` directive;

- using the ``literalinclude`` directive.

In all cases, Syntax highlighting is provided by `Pygments <https://pygments.org/>`_.
When using literal blocks, this is configured using any ``highlight`` directives in the source file.
When a ``highlight`` directive is encountered, it is used until the next highlight directive is encountered.
If there is no ``highlight`` directive in the file, the global highlighting language is used.
This defaults to ``python`` but can be configured using the ``highlight_language`` config value in ``config.py``.

The following values are supported:

- none (no highlighting)

- default (similar to ``python3`` but with a fallback to ``none`` without warning highlighting fails; the default when ``highlight_language`` isn’t set)

- guess (let Pygments guess the lexer based on contents, only works with certain well-recognizable languages)

- python

- rest

- c

- … and any other `lexer alias that Pygments supports <https://pygments.org/docs/lexers>`_

--------------------

``..highlight::`` language
    用于设置 :ref:`rst-literal-blocks` 高亮显示代码块。 例如::

        .. highlight:: python

    这一语言会一直使用直到下一个 ``.. highlight::`` 指令出现。

    .. rubric:: Options

    ``:linenothreshold:`` *n (int)*
        超过这个行数的代码块将会显示行号。

    ``:force:`` *(no value)*
        If given, minor errors on highlighting are ignored.

---------------------

``.. code-block::`` [language]
    与 ``.. code::`` 及 ``.. sourcecode::`` 没什么太大的区别，都用于显示代码块。
    

    例如::

        .. code-block:: python

            def my_function():
                "just a test"
                print("Hello, World!")
    
    .. rubric:: Options
    
    ``:linenos:`` *(no value)*
        启动代码块显示行号。

    ``:lineno-start:`` *number (number)*
        设置行号的起始值。

    ``:emphasize-lines:`` *line numbers (comma separated numbers)*
        突出显示指定行。

    ``:force:`` *(no value)*
        忽略高亮显示代码块时的小错误。

    ``:caption:`` *text (text)*
        为代码块添加标题。

    ``:name:`` *label (text)*
        为代码块添加一个可被 ``ref`` 引用的标靶名。

    ``:class:`` *class names (a list of class names, separated by spaces)*
        为代码块添加CSS样式。

    ``:dedent:`` *number (number or no value)*
        删除代码块中的缩进字符，可以是一个数字，也可以是没有值，没有值时空格将由 :func:`textwrap.dedent` （见 `python3.13 <https://docs.python.org/3/library/textwrap.html#textwrap.dedent>`_ ）
        自动删除。

-----------------------

``.. literalinclude::`` filename
    从文件中导入代码块。文件名一般是相对于当前文件的路径，当若要用绝对路径，根目录为顶层的source dierctory。

    .. rubric:: Options
    
    详情见 `sphinx 文档 <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-literalinclude>`_ 。


==================
Toctree
==================

``.. toctree::`` 是一个sphinx特有的指令，用于生成目录。 通常用于生成文档的主目录。例如::

    .. toctree::
       :maxdepth: 2
       :caption: 目录:

       introduction
       usage
       advanced

其中 content 为文件名，不带扩展名，相对于当前文件的路径。

.. rubric:: Options

``:class:`` *class names (a list of class names, separated by spaces)*
    为目录元素添加CSS样式，这是一个 `common option <https://docutils.sourceforge.io/docs/ref/rst/directives.html#common-options>`_ 。

``:name:`` *label (text)*
    为目录元素添加一个可被 ``ref`` 引用的标靶名，这是一个 `common option <https://docutils.sourceforge.io/docs/ref/rst/directives.html#common-options>`_ 。

``:caption:`` *(text)*
    为目录添加一个标题。

``:numbered:`` *depth (integer)*
    为目录元素添加编号。

``:maxdepth:`` *level (integer)*
    限制目录的深度。

``:hidden:`` 
    隐藏目录。

``:glob:`` 
    使用通配符匹配 ``*`` 文件名。
    例如： ``intro*`` 匹配所有以 ``intro`` 开头的文件。

``:includehidden:`` 
    包括隐藏的文件。

``:titlesonly:`` 
    只显示标题。

``:includehidden:`` 
    包括隐藏的文件。

``:titlesonly:`` 
    只显示文档标题（titles），不显示其他同级标题（heading）。

``:reversed:`` 
    反转目录。


==================
Include
==================

``.. include::`` 指令可以将外部文档导入进来。

将外部文档作为 reStructuredText 导入，使用相同的渲染方式::

    .. include:: path/to/document.rst

相对路径的起点是当前文档所在的文件夹。

.. rubric:: Options

start-line
    整数，从文件的第几行开始读取。 和 Python 一样，第一行的索引值是 0 。

end-line
    整数，到文件的第几行结束。 第一行零。

start-after
    字符串，将从文件中第一次找到的指定字符串后开始读取。

end-before
    字符串，将在文件中第一次找到的指定字符串前结束。

encoding
    字符编码。

literal
    是否以纯文本字面量的形式导入。

code
    输入 Pygments 支持的分词器名，以指定语言的词法规则将导入内容格式化。

number-lines
    整数。设置第一样的行号。仅在 literal 或 code 选项被设置时工作。
    
tab-width
    整数。设置制表符所渲染的空格的数目。仅在 literal 或 code 选项被设置时工作。

.. _rst-directive-raw:

====================
Raw
====================

``.. raw::`` 指令将其内容在指定的输出格式下原样传递给输出::

    .. raw:: html

        <hr width=50 size=10>

.. rubric:: Options

file
    从文件系统中读取内容。

url
    从网络读取内容。

encoding
    外部内容的字符编码。

另外可参考 :ref:`rst-role-raw` 角色。


====================
Class
====================

``.. class::`` 指令用于声明其内容或接下来的内容的类型。 对于 HTML 输出而言，这会在其内容的 class 属性中添加指定的名称。

.. code-block:: rst

    .. class:: special

    一个『特殊的』段落。

    .. class:: exceptional remarkable

    一个『例外』的章节
    ==================

    这是个普通段落。

    .. class:: multiple

        如果要传递内容，那么需要是体元素。
        比如一个或多个段落。

        这是第二段。

上面的渲染效果用一段伪 XML 来表示::

    <paragraph classes="special">
        一个『特殊的』段落。
    <section classes="exceptional remarkable">
        <title>
            一个『例外』的章节
        <paragraph>
            这是个普通段落。
        <paragraph classes="multiple">
            如果要传递内容，那么需要是体元素。比如一个或多个段落。
        <paragraph classes="multiple">
            这是第二段。


====================
Glossary
====================

``.. glossary::`` 指令用于定义术语表。

This directive must contain a reStructuredText definition-list-like markup with terms and definitions.
The definitions will then be referenceable with the ``term`` role.

例子::

    .. glossary::

    environment
        A structure where information about all documents under the root is
        saved, and used for cross-referencing.  The environment is pickled
        after the parsing stage, so that successive runs only need to read
        and parse new and changed documents.

    source directory
        The directory which, including its subdirectories, contains all
        source files for one Sphinx project.

效果：

.. glossary::

    environment
        A structure where information about all documents under the root is
        saved, and used for cross-referencing.  The environment is pickled
        after the parsing stage, so that successive runs only need to read
        and parse new and changed documents.

    source directory
        The directory which, including its subdirectories, contains all
        source files for one Sphinx project.

不同于一般的定义列表，每个条目允许多个术语，术语中允许内联标记。你可以链接到所有的术语。例如::

    .. glossary::

        term 1
        term 2
            Definition of both terms.

.. rubric:: Options

``:sorted:``
    Sort the entries alphabetically.


====================
Math
====================

``.. math::`` 指令用于在文档中插入数学公式。

输入的数学语言是 LaTeX 标记。这是纯文本数学标记的事实标准，而且在构建 LaTeX 输出时不需要进一步的翻译。

该指令支持多个等式，但需要用空行分隔::

    .. math::

        (a + b)^2 = a^2 + 2ab + b^2

        (a - b)^2 = a^2 - 2ab + b^2

效果：

.. math::

    (a + b)^2 = a^2 + 2ab + b^2

    (a - b)^2 = a^2 - 2ab + b^2

此外，每个单独的等式都在一个分割环境中，
这意味着你可以在一个等式中有多个对齐的行，用 ``&`` 对齐，用 ``\\`` 分隔::

    .. math::

        (a + b)^2  &=  (a + b)(a + b) \\
                   &=  a^2 + 2ab + b^2

效果：

.. math::

    (a + b)^2  &=  (a + b)(a + b) \\
               &=  a^2 + 2ab + b^2

当数学只有一行文本时，也可以作为指令参数给出::

    .. math:: (a + b)^2 = a^2 + 2ab + b^2

.. rubric:: Options

``:class:`` *class names (a list of class names, separated by spaces)*
    为数学公式添加CSS样式。

``:name:`` *label (text)*
    为数学公式添加一个可被 ``ref`` 引用的标靶名。

``:label:`` *label (text)*
    一般情况下，数学公式不会被编号。如果你想让你的公式获得一个编号，使用 ``label`` 选项。
    当使用时，它为等式选择一个内部标签，通过它可以进行交叉引用，并导致发出一个等式编号。
    参见 :ref:`eq <rst-role-math>` 角色的示例。编号样式取决于输出格式。
    
``:nowrap:``
    阻止给定数学公式在数学环境中自动换行。当给出此选项时，你必须自己确保数学公式正确设置。例如::

        .. math::
            :nowrap:

            \begin{eqnarray}
                y    & = & ax^2 + bx + c \\
                f(x) & = & x^2 + 2xy + y^2
            \end{eqnarray}
    
    效果：

    .. math::
        :nowrap:

        \begin{eqnarray}
            y    & = & ax^2 + bx + c \\
            f(x) & = & x^2 + 2xy + y^2
        \end{eqnarray}

.. seealso::

    `Math support for HTML outputs in Sphinx <https://www.sphinx-doc.org/en/master/usage/extensions/math.html#math-support>`_
        Rendering options for math with HTML builders.

    `latex_engine <https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-latex_engine>`_
        Explains how to configure LaTeX builder to support Unicode literals in
        math mark-up.


====================
Role
====================

``.. role::`` 指令用于创建一个新的角色。
例如::

    .. role:: custom-role
        :class: my-custom-class

    这是一个 :custom-role:`自定义角色` 的示例。

然后在CSS文件中定义 ``my-custom-class`` 类的样式：

.. code-block:: css

    .my-custom-class {
        color: red;
        font-weight: lighter;
    }

定义必须在使用的前面。你还可以用一个括号来表达继承关系::

    .. role:: custom-role(strong)

    继承了:custom:`强调`。

特例是 ``raw`` 角色，她不能直接只用，必须创建一个新角色来继承她，可见 :ref:`rst-role-raw`::

    .. role:: html(raw)
        :format: html

    在 HTML 中，将会渲染
    :raw-html:`<ruby><rb>拼</rb><rt>pin</rt><rb>音</rb><rt>yin</rt></ruby>`

.. rubric:: Options

``:class:`` *class names (a list of class names, separated by spaces)*
    为角色添加CSS样式。

``:format:`` *space-separated list of output format names (writer names)*
    为角色指定输出格式，支持的格式见 `writer names <https://docutils.sourceforge.io/docs/user/config.html#writer-docutils-application>`_。

``:language:`` *text*
    设置高亮的代码名。

-----------------------

``.. default-role::`` 指令用于设置的解释文本角色的行为::

    .. default-role:: math

    现在是数学角色了： `\LaTeX`

    .. default-role:: literal

    `现在是字面量了`

.. default-role:: math

效果：

现在是数学角色了： `\LaTeX`

.. default-role:: literal

`现在是字面量了`


====================
HTML Metadata
====================

``.. meta::`` 指令允许指定 Sphinx 文档页面的 HTML `metadata element`_。

For example, the
directive::

   .. meta::
      :description: The Sphinx documentation builder
      :keywords: Sphinx, documentation, builder

will generate the following HTML output:

.. code-block:: html

   <meta name="description" content="The Sphinx documentation builder">
   <meta name="keywords" content="Sphinx, documentation, builder">

同样的，Sphinx 会根据 ``meta`` 指令中的 ``lang`` 属性将关键字添加到搜索索引中::

   .. meta::
      :keywords: backup
      :keywords lang=en: pleasefindthiskey pleasefindthiskeytoo
      :keywords lang=de: bittediesenkeyfinden

这个例子将以下关键字添加到不同语言配置的构建的搜索索引中：

* ``pleasefindthiskey``, ``pleasefindthiskeytoo`` to *English* builds;
* ``bittediesenkeyfinden`` to *German* builds;
* ``backup`` to builds in all languages.

.. _metadata element: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta

------------------------

``.. title::`` 指令用于覆盖文档的标题。

这个指令会以元数据的形式指定文档标题，不会成为文档主体的一部分。
同时会覆盖文档提供的标题（ `document title`_ ）和 ``title`` 配置设置（ `"title" configuration setting`_ ）。

例如::

    .. title:: New Sphinx Documentation Title

可见标签页的标题会变成 ``New Sphinx Documentation Title``。

.. _document title: https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#document-title
.. _"title" configuration setting: https://docutils.sourceforge.io/docs/user/config.html#title