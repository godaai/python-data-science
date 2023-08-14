# Python数据科学实战![Static Badge](https://img.shields.io/badge/python-3.8%2B-blue)![Github stars](https://img.shields.io/github/stars/py-101/python-data-science.svg)


开源的、面向人文社科的 Python 数据科学教程。

本项目内容包括数据分析、可视化、机器学习与因果推断。

在线阅读地址：https://py-101.github.io/python-data-science/

如果这个项目对你有帮助，欢迎 star本仓库。



<img src= "img\ch-pyhton-lang\python-ecosystem.svg" height= 300>



## 本教程的适用群体

本书适用于以下读者：

1. 接触过基础 python 语法，期望学习数据科学、机器学习的经管背景读者；
2. 寻找数据分析实战教程的读者；
3. 对数据应用和前沿因果推断问题感兴趣的读者。

## 主要内容

#### 科学计算

* NumPy array

#### 数据处理

* pandas DataFrame。

* 数据读取与预处理。

#### 数据可视化

- matplotlib 

#### 机器学习

* scikit-learn

#### 因果推断

#### 深度学习

## 参与编写

本书基于名为 d2lbook 的 Python 工具编译，并部署在 GitHub Pages 上。

您可以通过网站[在线阅读](https://py-101.github.io/python-data-science/)并复现书中内容。

或者您可以通过以下方式本地预览。

### 克隆仓库

通过git clone的方式克隆本仓库到本地

```git
git clone https://github.com/py-101/python-data-science.git
```


### 环境配置

本书基于名为 d2lbook 的 Python 工具编译，书中案例基于python3.8

* 选择一个包管理工具，比如 `conda` 或者 `venv`
* 安装 Python >= 3.8
* 安装 requirements.txt 中的各个依赖。包括本书各个案例所需要的工具 pandas 等，以及本电子书构建工具 d2lbook：

```bash
pip install -r requirements.txt
```

### 使用 d2lbook 构建

[d2lbook]((https://book.d2l.ai/)) 主要功能是将 markdown 文件编译成各个中间文件，包括 `.ipynb` 和 `.html`。 `_build` 目录下面几个文件夹主要功能:

* `eval` 文件夹将 `.md` 文件转换为 `.ipynb`，并且运行这个 `.ipynb`，各个代码段均有运行结果。
* `rst` 文件夹将 `.ipynb` 文件转换为 `.rst` 文件。
* `html` 文件夹将 `.rst` 文件转换为 HTML。

但实际使用起来，d2lbook 有一些小问题。d2lbook 使用了名为 [notedown](https://github.com/d2l-ai/notedown/) 工具，将 `.md` 文件运行，并转化为 `.ipynb` 文件，在调用 notedown 时，d2lbook 只使用了1个CPU核心，速度比较慢，如果每个 `.md` 文件都从零开始运行并构建，时间很长。

解决办法：将 `_build/eval/` 内容也加进了 git 仓库，每次只对有改动的 `.md` 文件转换为 `.ipynb` 并运行，例如，修改了 `ch-xxxx/yyyy.md`，构建时，调用该命令，重新生成对应的 `.ipynb`：

```bash
notedown ch-xxxx/yyyy.md --run --timeout=1200 > _build/eval/ch-xxxx/yyyy.ipynb
```

构建 HTML 时，不需要运行 `.ipynb` 里面的 Python 代码， 只需从 `.ipynb` 转化为最终的 HTML。下面的命令从已经有运行结果的 `.ipynb` 转化为 HTML，并拷贝到 `docs` 目录：

```bash
sh build_from_eval_ipynb.sh
```

如果想从 `.md` 文件开始构建工程，可以考虑使用 `build_from_scratch.md`，由于刚提到的只能使用1个CPU核的问题，速度比较慢，部分 `.md` 文件可能无法生成 `.ipynb`：

```bash
sh build_from_scratch.md
```

### 部署到 GitHub Pages

本项目的 HTML 部署在 GitHub Pages 上，GitHub Pages 读取 `docs` 目录下内容。`build_from_eval_ipynb.sh` 脚本最后几行就是将最新生成的 HTML 更新到 `docs` 目录。

### 启动 HTTP Server

构建好 HTML 文件后，如果是在自己的个人电脑，可以使用 Python 自带的 HTTP Server，并在浏览器里打开 http://127.0.0.1:8000 查看效果。

```bash
cd docs
python -m http.server 8000
```

### 图片

图片建议下载并使用 [drawio](https://www.drawio.com/) 绘制，或者[在线](https://app.diagrams.net/)绘制，`.drawio` 绘图文件保存到项目 `drawio` 文件夹下，svg 图片保存到 `img` 文件夹下。

### 构建 PDF

构建 pdf 时如果有 svg 图片需要安装 LibRsvg 来转换 svg 图片，安装 `librsvg` 可以通过`apt-get install librsvg`（如果是 macOS 可以用 Homebrew）。

构建 pdf 必须要有 LaTeX，请安装 [Tex Live](https://www.tug.org/texlive/)。

## 贡献

欢迎您通过issue的方式来报告教程中的问题：[issue](https://github.com/py-101/python-data-science/issues)

## LICENSE
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-lightgrey" /></a>

本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。
