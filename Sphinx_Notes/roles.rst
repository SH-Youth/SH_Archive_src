###########################
Roles
###########################

.. image:: https://moe-counter.sai-hentai.dpdns.org/blog-sphinxnote-roles/
   :alt: 访问量统计

A role or "custom interpreted text role" is an inline
piece of explicit markup. It signifies that the enclosed text should be
interpreted in a specific way.  Sphinx uses this to provide semantic markup and
cross-referencing of identifiers, as described in the appropriate section.  The
general syntax is ``:rolename:`content```.

Docutils supports the following roles:

* `emphasis` -- equivalent of ``*emphasis*``
* `strong` -- equivalent of ``**strong**``
* `literal` -- equivalent of ````literal````
* `subscript` -- subscript text
* `superscript` -- superscript text
* `title-reference` -- for titles of books, periodicals, and other materials


上下标
    例如::

        H\ :sub:`2` O
        E = mc\ :sup:`2`

    效果:

        | H\ :sub:`2` O
        | E = mc\ :sup:`2`

解释性文本角色
    即一对单反引号所包括的文本角色。它默认指 ``title references`` ，
    但可以用一个 `default-role` 指令更改它在接下来的文本处理中的行为::

        .. default-role:: math

        现在是数学角色了： `\LaTeX`

        .. default-role:: literal

        `现在是字面量了`

    .. default-role:: math

    现在是数学角色了： `\LaTeX`

    .. default-role:: literal

    `现在是字面量了`


===========================
Inline code highlighting
===========================

``:code:``
    An inline code example. When used directly, this role just displays the text without syntax highlighting, as a literal.
   
    Unlike the code-block directive, this role does not respect the default language set by the highlight directive.
   
    To display a multi-line code example, use the code-block directive instead.

    `code` 角色::

        :code:`println!("{}", 8usize)`

    效果：
        :code:`println!("{}", 8usize)`

    默认的代码角色是回退到 literal 的，没有高亮，
    可以通过 role 指令创建新的代码角色以启用高亮::

        .. role:: python(code)
            :language: python

        In Python, :python:`1 + 2` is equal to :python:`3`.

    效果:
        .. role:: python(code)
            :language: python

        In Python, :python:`1 + 2` is equal to :python:`3`.

.. _rst-role-math:

---------------------------
Mathematics
---------------------------

``:math:``
    This role is used to display mathematical notation. It is equivalent to the math directive.
    
    根据生成器的配置，可能使用 MathJax, KaTeX 渲染，
    或者使用本地 LaTeX 编译成 SVG 嵌入。

    这个得看生成器的配置，docutils 只管语义表达。
    Sphinx 默认使用 MathJax，但是推荐使用以下设置配置成 :math:`\KaTeX`，
    比前者性能高出不少。
    例如::

        The Pythagorean theorem is expressed as :math:`a^2 + b^2 = c^2`.

    效果:
        The Pythagorean theorem is expressed as :math:`a^2 + b^2 = c^2`.

``:eq:``
   Same as ``math:numref``.

   例如::

      .. math:: e^{i\pi} + 1 = 0
         :label: euler

      Euler's identity, equation :math:numref:`euler`, was elected one of the
      most beautiful mathematical formulas.

   效果：
      .. math:: e^{i\pi} + 1 = 0
         :label: euler

      Euler's identity, equation :math:numref:`euler`, was elected one of the
      most beautiful mathematical formulas.

.. _rst-role-raw:

---------------------------
Raw
---------------------------

`raw` 角色，表示将内容原封不动地传递给输出。
这个角色不能直接使用，而是使用 `role` 指令定义一个新角色，并指定输出格式::

    .. role:: html(raw)
        :format: html

这样，将会限制其只在 html 输出格式下以原始文本渲染该角色的内容，而在其他输出格式下，将如同注释一般不会渲染。

例如::

    .. role:: raw-html(raw)
        :format: html

    .. role:: raw-latex(raw)
        :format: latex

    在 HTML 中，将会渲染
    :raw-html:`<ruby><rb>拼</rb><rt>pin</rt><rb>音</rb><rt>yin</rt></ruby>`，
    而 :raw-latex:`怎么做哦` 应该是不会在 HTML 输出中渲染的。

.. role:: raw-html(raw)
    :format: html

.. role:: raw-latex(raw)
    :format: latex

在 HTML 中，将会渲染
:raw-html:`<ruby><rb>拼</rb><rt>pin</rt><rb>音</rb><rt>yin</rt></ruby>`，
而 :raw-latex:`怎么做哦` 应该是不会在 HTML 输出中渲染的。

另外可参考 :ref:`rst-directive-raw` 指令。


===========================
Cross-references
===========================


---------------------------
交叉引用任意位置
---------------------------

``:ref:``
   To support cross-referencing to arbitrary locations in any document, the standard reStructuredText labels are used. For this to work label names must be unique throughout the entire documentation. There are two ways in which you can refer to labels:
   
   1. 如果将标签直接放在节标题之前，则可以使用 ``:ref:`label name``` 来引用它。例如::

         .. _my-reference-label:

         Section to cross-reference
         --------------------------

         This is the text of the section.

         It refers to the section itself, see :ref:`my-reference-label`.

      然后， ``:ref:`` 角色将生成一个到节的链接，链接标题为“要交叉引用的节”。当节和引用位于不同的源文件中时，这种方法同样有效。
      
      自动标签也适用于图形。例如::
      
         .. _my-figure:

         .. figure:: whatever

            Figure caption
      
      在这种情况下，引用 ``my-figure`` 会插入一个引用到图，链接文本为“figure caption”。

   2. 没有放在节标题之前的标签仍然可以被引用，但是必须给链接一个显式的标题，使用以下语法：“Link title”。例如::

         .. _label name:

         I love rst.

         Yeap, I know :ref:`Link title <label name>`.

      .. _label name:

      I love rst.

      Yeap, I know :ref:`Link title <label name>`.


---------------------------
交叉引用文件
---------------------------

``:doc:``
    角色用于链接到指定的文档，可以使用绝对路径或相对路径来指定文档名称。

    1. **绝对路径引用**：
       ``:doc:`/absolute/path/to/document```

    2. **相对路径引用**：
       ``:doc:`relative/path/to/document```

    **示例用法:**

    假设你有以下目录结构::

        /docs
            /index.rst
            /section
                /subsection
                    /target.rst

      
    在 ``index.rst`` 中引用 ``target.rst``：
    ``:doc:`/section/subsection/target````

    在 ``section/subsection/another.rst`` 中引用 ``target.rst``：
    ``:doc:`target````

    如果没有给出明确的链接文本，链接标题将是目标文档的标题。要给出明确的链接文本，可以使用以下语法::

        :doc:`Link title </path/to/document>`


--------------------------------
图号对照图
--------------------------------

``:numref:``
    角色用于生成带编号的引用，通常用于引用图表、表格、代码块等带编号的元素。

    示例用法

    假设你有一个带编号的图表::

        .. _my-figure:

        .. figure:: /path/to/image.png
            :alt: My Figure

            Figure caption

    你可以使用 ``:numref:`` 来引用这个图表::

        See :numref:`Figure <my-figure>`.

    这样会生成一个带编号的引用，链接文本为 "Figure 1"（假设这是文档中的第一个图表）。


-----------------------------
引用可下载文件
-----------------------------

``:download:``
    当您使用此角色时，被引用的文件将自动标记为在生成时包含在输出中（显然，仅用于HTML输出）。所有可下载的文件都放在输出目录的 ``_downloads/<unique hash>/`` 子目录中；处理重复的文件名。

    例如::

        See :download:`this example script <../example.py>`.

    给定的文件名通常是相对于当前源文件所在的目录的，但是如果是绝对的（以 ``/`` 开头），则将其视为相对于顶级源目录的文件名。

    在 ``example.py`` 文件将被复制到输出目录，并为其生成适当的链接。

    若要不显示不可用的下载链接，应将具有此角色的整个段落换行::
      
        .. only:: builder_html

            See :download:`this example script <../example.py>`.


-----------------------------
其他语义标记
-----------------------------

以下角色除了以不同的样式格式化文本外，不会执行任何特殊操作：

``:abbr:``
    缩写。如果角色内容包含带圆括号的解释，则将对其进行特殊处理：它将在HTML的工具提示中显示，在LaTeX中仅输出一次。

    例如: ``:abbr:`LIFO (last-in, first-out)``` ，效果: :abbr:`LIFO (last-in, first-out)`。

``:file:``
    角色用于创建指向文件的超链接。示例用法：

    假设你有一个文件 ``example.txt``，你可以这样引用它::

        See the file :file:`example.txt` for more details.

    如果你想给链接一个明确的标题，可以使用以下语法::

        See the :file:`example file <example.txt>` for more details.

    这样会生成一个链接，链接文本为 "example file"，指向 ``example.txt`` 文件。
