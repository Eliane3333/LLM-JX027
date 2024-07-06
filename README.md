# 基于ollama平台进行大语言模型初探 
## 本项目利用ollama平台、docker、open webUI等工具部署可离线或在内部网络中使用的大语言模型，并进行交互界面优化；以gemma2模型为例，使用uncloth进行初步微调。 
## 环境依赖
windows10操作系统笔记本
在本实验的前期对笔记本配置并无较高要求。后期运行离线大模型时需要您有较大内存以及较高配置。
使用前请您勾选“启用或关闭windows功能”中的“适用于Linux的Windows子系统”和“虚拟机平台”两项

## 1、安装ollama
[ollama安装github项目地址](https://github.com/ollama/ollama?tab=readme-ov-file)
 ### 下载所对应的版本 我安装的是windows版本。
### 1.1 下载完成后解压，得到setup程序，运行，默认安装路径是C盘。 
### 1.2 为了防止资源紧张，更改环境变量。
 -  在我的电脑-右键-属性中的高级系统设置找到环境变量。 系统环境变量名OLLAMA_MODELS，值是你要把大模型安装的位置，例如：`E:\\ollama`。
### 1.3 使用说明 
 - 命令行定位到ollama的位置，直接运行 `ollama run llama3` 命令即可安装llama3模型，安装完成后即可在命令行中开始对话。
但这种方式不太美观并且使用较为麻烦

## 2、交互界面优化
### **2.1 安装wsl** 
以管理员身份运行命令提示符。 输入：
`wsl --set-default-version 2`
 `wsl --update --web-download`
 
### **2.2 安装docker** 
国内镜像：[docker github地址](https://github.com/tech-shrimp/docker_installer) 
下载Windows版本安装包，更改路径安装。在命令提示符中输入：`start /w "" "Docker Desktop Installer.exe" install --installation-dir=E:\docker` 
之后按照提示进行安装，安装好后进入注册界面，这里可以选择跳过不注册。

### **2.3 安装open webUI** 
进入[open webUI官网](https://docs.openwebui.com/)
根据提示打开cmd输入以下命令： 
``` docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main ```
等待安装完毕后，回到docker界面，可看到出现了open webUI的链接。
访问[http://localhost:3000](http://localhost:3000/)，或者直接点击链接，进入webUI的登录界面，注册。
这里的注册信息会保存在本地主机，不会上传到云端，且第一个注册的用户会成为管理员，可以管理其他的用户。


### 2.4 开始探索ollama
在ollama的library中有很多可用的模型，比如llama3、gemma2、qwen2等
复制模型名，粘贴到settings-Admin settings-models，等待下载完成即可使用


 ## 3、模型选择与微调
  
 
### 3.1 选择模型，如qwen2（通义千问）
[https://ollama.com/library/qwen2](https://ollama.com/library/qwen2)
下载完成后在对话界面选择通义千问模型。测试离线效果，将电脑设置成为飞行模式后测试模型还能否正常回答。
对于训练时使用英文的模型，比如llama等，可以在ollama中设置系统提示词，让模型使用中文回答问题。
 ### 3.2 选择模型助手，如Foto Forge。 
 Foto Forg是一个图像处理领域的模型助手，在[https://openwebui.com/#open-webui-community](https://openwebui.com/#open-webui-community)这里可以找到它，
[foto-forge:latest](https://openwebui.com/m/dwrou/foto-forge:latest/)详细信息
 ### 3.3 模型微调
  模型微调有两种方式
  - 在workspace-document中添加文件，让大模型在回答前进行检索，但这种不算严格意义上的微调，只是用文件弥补了大模型的知识库局限性，大模型每次回答问题时都要检索文件再生成，消耗的时间增多。
   - 模型微调，将知识内化整合成模型的一部分。 
   - - 准备json问答范例供模型进行学习。 将一个文件全部转化为json格式需要大量时间和精力，可借助通义千问帮助进行这项工作。
   -  - 进入uncloth网站：[https://github.com/unslothai/unsloth](https://github.com/unslothai/unsloth) 
   - - 通过uncloth打开要微调的模型的Colab笔记[Google colab](https://colab.research.google.com/drive/1vIrqH5uYDQwsJ4-OO3DErvuv4pBgVwk4?usp=sharing)，colab基于google，访问需要魔法。 
   - - 将json文件添加到colab的编辑空间中，更改`Data Prep` 模块中读取资料的代码，将其路径改为我们添加的json文件。 
  找到GGUF开头的区块，这个区块用来指定如何储存微调好的语言模型，修改相关信息。
  - - 训练过程需要大量时间，但训练好的新模型可以直接导入ollama。
 
 ## 
