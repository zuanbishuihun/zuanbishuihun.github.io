---
title: "部署Hugo Doit主题（一）原始框架搭建"
subtitle: ""
date: 2024-08-07T19:37:39+08:00
lastmod: 2024-08-07T19:37:39+08:00
draft: false
authors: []
description: "这是我个人搭建Hugo DoIt博客的步骤流程系列，包含部署以及配置site config、Gitpages + action、seo、域名等相关参数，希望可以帮助解决一些部署上的问题，也是为自己留下搭建流程作为记录"

tags: ["Hugo","DoIt"]
categories: ["安装文档"]
series: ["个人博客"]

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---

<!--more-->

简要介绍一下Hugo DoIt主题的部署流程，这里主要是为了记录一下部署的流程，以便日后查阅。在正式开始部署之前，首先需要感谢Hugo DoIt主题的作者们，这是[官方的Github地址DoIt](https://github.com/HEIGE-PCloud/DoIt)。另外也要感谢Hugo项目，Hugo是由Go语言实现的静态网站生成器，可以通过[快速入门文档](https://gohugo.io/getting-started/quick-start/) 进一步了解Hugo。

## 部署hugo DoIt博客原始框架

这部分需要参照[DoIt官方详细搭建流程](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)进行操作，需要安装相关的软件，配置相关的环境，具体的操作步骤如下：

### 1.安装hugo

{{< admonition tip "推荐使用 Hugo extended 版本" >}}
由于这个主题的一些特性需要将 :(fab fa-sass fa-fw): SCSS 转换为 :(fab fa-css3 fa-fw): CSS, 推荐使用 Hugo **extended** 版本来获得更好的使用体验.
{{< /admonition >}}

原部署流程中说明了Hugo extended版本的重要性，因此最好肯定安装这个版本。本人使用的Linux系统，因此直接在snap中安装是最便捷的，直接使用以下命令即可：

```shell
sudo snap install hugo
```

安装完成后可以查看hugo版本，确实也是extended版本，版本维护也非常新，非常方便。

```shell
hugo version             
hugo v0.131.0-bfbee17932ff24009008aa94cdd75c0c41f59279+extended linux/amd64 BuildDate=2024-08-02T09:03:48Z VendorInfo=snap:0.131.0
```

{{< admonition warning "apt等Linux自带软件包的hugo版本可能不符合本hugo主题" >}}
笔者测试了Ubuntu22.04版本的apt，其中的hugo版本是较老的，并且不是extended版本，因此建议使用snap安装hugo。但是Ubuntu是自带了snap,因此如果是Debian或者其他系统，可能需要自行安装snap。
{{< /admonition >}}

### 2.在本地创建个人博客项目并使用DoIt主题

这部分需要在本地创建一个hugo项目，然后使用DoIt主题，也就是说以后的博客内容都是在这个项目中进行编写的。具体的操作步骤如下：

#### 2.1 创建项目

Hugo 提供了一个 `new` 命令来创建一个新的网站，`hugo new site`后跟的是项目名称这，名称不会对项目配置造成任何影响，根据自己喜好命名即可，这里以`blog_test`为例：

```shell
hugo new site blog_test
cd blog_test
```

接下来可以观察一下此时的项目结构，可以看到项目中已经有了一些文件，这些文件是Hugo默认生成的，后续会根据DoIt主题的要求进行修改。而接下来，我们就应该导入DoIt主题。

```shell
(base)  zbl@zbl  ~/MyBlog/blog_test  tree
.
├── archetypes
│   └── default.md
├── assets
├── content
├── data
├── hugo.toml
├── i18n
├── layouts
├── static
└── themes
```

#### 2.2 导入DoIt主题

**DoIt** 主题的仓库是: [https://github.com/HEIGE-PCloud/DoIt](https://github.com/HEIGE-PCloud/DoIt).

你可以下载主题的 [最新版本 :(far fa-file-archive fa-fw): .zip 文件](https://github.com/HEIGE-PCloud/DoIt/releases) 并且解压放到 `themes` 目录.

另外, 也可以直接把这个主题克隆到 `themes` 目录:

```bash
git clone https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```

或者, 初始化你的项目目录为 git 仓库, 并且把主题仓库作为你的网站目录的子模块:

```bash
git init
git submodule add https://github.com/HEIGE-PCloud/DoIt.git themes/DoIt
```

{{< admonition tip "" >}}
以上是DoIt官方推荐的三种导入该主题的方式，我个人使用的是直接`git clone`的方式，这样可以保证主题的更新比较方便。不过怎么方便怎么来，总之就是把DoIt这个主题导入到项目的`themes` 目录下，这相当于是我们个人博客的模板。
{{< /admonition >}}

#### 2.3 配置主题

{{< admonition warning "自行配置主题" >}}
在这一阶段笔者并没有按照官方的教程，因为DoIt也是在LoveIt等主题上进行的扩充修改，因此在配置上也是有一些不同的地方，所以其实最直接的配置主题方式应该是找到`themes/DoIt/exampleSite`目录下的配置文件，然后复制到项目的根目录下，然后进行修改。我认为这样才是最完备的。
{{< /admonition >}}

自然最方便的就是将整个文件夹复制过去，首先保证自己在根目录下，然后复制，操作如下：

```shell
cp -r themes/DoIt/exampleSite/* .
```

之后就是进行具体的配置了，此处的配置涉及到很多方面，包括主题的配置，网站的配置，SEO的配置等等，这些都是在`config.toml`文件中进行配置的，具体的配置可以参考[DoIt官方配置文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)，之后关于部分细节再专门进行补充。但是目前我们已经搭建好了基本的使用框架。

#### 2.4 编写第一篇文章并启动！

{{< admonition warning "" >}}
请注意，在对个人博客项目进行任何操作时，你所在的地址一定要是博客项目的**根目录**！！！这样hugo执行的命令才是在你的工作目录下。
{{< /admonition >}}

在配置完成后，我们就可以编写第一篇文章了，这里以`hugo new posts/first.md`为例，创建一个`posts`目录，然后在里面创建一个`first.md`文件。需要注意的是，创建的md文件模板在项目根目录的`archetypes/default.md`中，这是笔者使用的模板：

```
---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
subtitle: ""
date: {{ .Date }}
lastmod: {{ .Date }}
draft: false
authors: []
description: ""

tags: []
categories: []
series: []

hiddenFromHomePage: false
hiddenFromSearch: false

featuredImage: ""
featuredImagePreview: ""

toc:
  enable: true
math:
  enable: false
lightgallery: false
license: ""
---

<!--more-->

TODO
```

创建完成后，执行指令`hugo server -e production`，将在本地启动一个服务，然后在浏览器中输入`http://localhost:1313/`即可看到你的博客页面，这是一个本地的页面，可以用来预览你的博客内容。

{{< admonition tip "pulic文件夹" >}}
hugo执行后，创建了一个`/public`文件夹，实际上这包含你网站的所有静态内容和资源，现在可以将其部署在任何 Web 服务器上，而在本系列的下一篇，就将这个文件夹部署到**Gitpages**上，同时为了更方便编写博客，将新写的博客集成到原本的项目中，并重新部署，我们将引入**Github actions**作为CI/CD工具。
{{< /admonition >}}

## 总结

这篇文章主要是记录了Hugo DoIt主题的部署流程，包括了Hugo的安装，项目的创建，主题的导入，主题的配置，第一篇文章的编写等，这是一个最基本的框架搭建，后续会继续进行更多的配置。

{{< admonition tip "参考文献" >}}
- [Hugo DoIt主题官方文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)
- [Hugo快速入门文档](https://gohugo.io/getting-started/quick-start/)
{{< /admonition >}}
