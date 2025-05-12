## 本地 Python 环境复制到目标服务器（目标服务器不联网）

将本地 Python 环境复制到不联网的目标服务器，主要策略是先在本地准备好所有需要的安装包，然后传输到目标服务器进行本地安装。

**步骤 1：在本地机器上生成 'requirements' 文件**

这个文件会列出你本地 Python 环境中所有已安装的包及其版本。打开你的终端或命令提示符，激活你想要复制的环境（如果是虚拟环境的话）。然后运行以下命令：

```bash
pip freeze > requirements.txt```

这会在你的当前目录下生成一个名为 requirements.txt 的文件。

** 步骤 2：在本地下载安装包**

接下来，我们使用 pip download 命令将 requirements.txt 中列出的所有包的 .whl 文件下载到本地。.whl 文件是 Python 的预编译包格式，方便安装。

```bash
pip download -r requirements.txt -d local_packages
-r requirements.txt: 告诉 pip 从 requirements.txt 文件中读取包列表。
-d local_packages: 指定下载的 .whl 文件保存的目录（你可以自定义这个目录的名称）。
执行这个命令后，所有在 requirements.txt 中列出的包（以及它们的依赖）都会被下载到 local_packages 文件夹中。

**步骤 3：将文件传输到目标服务器**

现在，你需要将以下内容通过 U 盘、网络共享（如果目标服务器在局域网内）、或者其他文件传输方式拷贝到目标服务器上：

你生成的'requirements.txt' 文件。
整个 local_packages 文件夹，包含所有下载的 .whl 文件。
**步骤 4：在目标服务器上安装包**

在目标服务器上，导航到你存放这些文件的目录。如果你想在目标服务器上创建一个独立的虚拟环境（强烈建议这样做以隔离依赖），可以执行以下命令：

```bash

# 如果你的目标服务器默认使用 Python 3
python3 -m venv venv
source venv/bin/activate  # 在 Linux 或 macOS 上
.\venv\Scripts\activate  # 在 Windows 上
进入你想要安装包的环境（或者直接在基础 Python 环境中，如果你不使用虚拟环境），然后使用 pip 命令进行本地安装，关键是使用 --no-index 和 --find-links 参数：

```bash

pip install --no-index --find-links="./local_packages" -r requirements.txt
--no-index: 告诉 pip 不要去 PyPI (Python Package Index) 寻找包。
--find-links="./local_packages": 告诉 pip 在 ./local_packages 这个目录下寻找安装包（请根据你实际存放 local_packages 文件夹的路径进行调整）。
-r requirements.txt: 告诉 pip 从 requirements.txt 文件中读取需要安装的包列表。
这条命令会指示 pip 在你提供的 local_packages 目录中查找所需的 .whl 文件，并进行安装。由于 --no-index 的存在，pip 不会尝试连接互联网。

## 总结一下步骤：

本地生成 requirements.txt。
本地使用 pip download 下载所有包到指定文件夹。
将 requirements.txt 文件和包含所有 .whl 文件的文件夹传输到目标服务器。
在目标服务器上使用 pip install --no-index --find-links="..." -r requirements.txt 进行本地安装。
这样，你就能在不联网的目标服务器上成功复制你的本地 Python 环境了。记得根据你的实际文件路径和环境情况调整命令。
