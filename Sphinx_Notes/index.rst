.. highlight:: rst

#############################
Sphinx 速查笔记
#############################

.. image:: https://moe-counter.sai-hentai.dpdns.org/blog-sphinxnote/
   :alt: 访问量统计

.. admonition:: 编者按

    这大概是我边学边整理的，本来基本上是来自Sphinx文档中 `reStructuredText <https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_ 
    的教程，但是实话实说那个写的确实一般，当我看得云里雾里的时候发现了 `Learn reStructureText <https://learn-rst.readthedocs.io/zh-cn/latest/index.html>`_
    这个写得确实不错，所以之后很多内容都改成了融合二者的写法，总体上有所综合、
    有所精简、有所补充，便于快速查阅，并且基本上将文档中的很多代码的实际表现效果展示了出来

    一些我觉得有用的指令或者角色都单独写成了一节放在 :ref:`reStructuredText+` ，还有一些我觉得也有点用的就放在指令和角色那一节里了。

    更多的细节在这里面找吧 https://docutils.sourceforge.io/docs/

.. toctree::
    :maxdepth: 1
    :caption: 其他章节：

    reStructuredText+
    roles
    directives


=============================
Installation and Quickstart
=============================

-----------------------------
Conda Install Sphinx
-----------------------------

使用以下命令在conda中安装Sphinx

.. code-block:: bash

    conda install sphinx

-----------------------------
Quickstart
-----------------------------

使用以下命令快速创建一个新的Sphinx项目

.. code-block:: bash

    sphinx-quickstart

按照提示进行配置即可

-----------------------------
Build
-----------------------------

**使用 `sphinx-build` 命令构建文档**

.. code-block:: bash

    sphinx-build -M html sourcedir builddir

其中 ``sourcedir`` 是源文件目录， ``builddir`` 是构建目录，
``-M`` 选项可以选择 builder，常用的除了 `html` 外还有 `latexpdf` ，
更多的builder可以查看 `Sphinx sphinx-build 命令文档 <https://www.sphinx-doc.org/en/master/man/sphinx-build.html#cmdoption-sphinx-build-M>`_

**使用 `make` 命令构建文档**

.. code-block:: bash

    make html
    make latexpdf

``sphinx-quickstart`` 生成的 ``Makefile`` 以及 ``make.bat`` 也可以用以构建文档。

**使用 `sphinx-autobuild` 命令实时构建文档**

.. code-block:: bash

    sphinx-autobuild sourcedir builddir

这个命令会在本地启动一个服务器，实时监控源文件的变化并自动构建文档。可以边写边看效果，非常方便。


=============================
Configuration
=============================

一个Sphinx项目的配置文件是 ``conf.py`` ，这个文件是一个Python脚本，用于配置Sphinx的各种选项。

-----------------------------
Project Information
-----------------------------

``project``
    项目的名称
    Example:

    .. code-block:: python

        project = 'blog'

``author``
    项目的作者
    Example:

    .. code-block:: python

        author = 'SHY'

``copyright``
    项目的版权声明

    允许的格式如下，其中 'YYYY' 代表一个四位数的年份。

    * ``copyright = 'YYYY'```
    * ``copyright = 'YYYY, Author Name'``
    * ``copyright = 'YYYY Author Name'``
    * ``copyright = 'YYYY-YYYY, Author Name'``
    * ``copyright = 'YYYY-YYYY Author Name'``

    如果版权声明中包含 :code:`'%Y'` ，那么它会被替换为当前的四位数年份。

    * ``copyright = '%Y'``
    * ``copyright = '%Y, Author Name'``
    * ``copyright = 'YYYY-%Y, Author Name'``

``version``
   项目的主要版本号，可以用于替换 ``|version|`` 的默认值。

``release``
    项目的完整版本号，可以用于替换 ``|release|`` 的默认值。

-----------------------------
General Configuration
-----------------------------

``extensions``
    一个列表，包含Sphinx扩展的名称。默认为空列表。
    这些拓展可以是Sphinx自带的（以 ``sphinx.ext.*`` 命名）或者自定义的第一方或第三方拓展。
    要使用第三方拓展，你必须确保它已经安装，并且在拓展列表中包含它。
    
    Example:

    .. code-block:: python

        extensions = [
            'sphinx.ext.todo',
            ...,
            'sphinx.ext.viewcode',
            'numpydoc',
        ]

    使用第一方拓展有两种方式。配置文件本身可以是一个拓展；对于这种情况，你只需要在其中提供一个 ``setup()`` 函数。
    否则，你必须确保你的自定义拓展是可导入的，并且位于Python路径中的一个目录中。

    确保在修改 sys.path 时使用绝对路径。如果你的自定义拓展位于相对于配置目录的目录中，请使用 pathlib.Path.resolve() 如下所示：

    .. code-block:: python

        import sys
        from pathlib import Path

        sys.path.append(str(Path('sphinxext').resolve()))

        extensions = [
        ...
        'extname',
        ]

    路径结构如下：

    .. code-block:: bash

        docs/
        ├── conf.py
        ├── index.rst
        └── sphinxext/
            └── extname.py

    更多关于拓展的信息请查看 `Sphinx 扩展文档 <https://www.sphinx-doc.org/en/master/usage/extensions/index.html>`_

