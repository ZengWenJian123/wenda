# 闻达：一个大规模语言模型调用平台
## 简介
一个LLM调用平台。旨在通过使用为小模型外挂知识库查找的方式，实现近似于大模型的生成能力。
1. 目前支持模型：`chatGLM-6B`、`chatRWKV`、`chatYuan`、`llama系列`。
2. 知识库扩展模型能力
3. 支持参数在线调整
4. 支持`chatGLM-6B`、`chatRWKV`、`llama系列`流式输出和输出过程中中断
5. 自动保存对话历史至浏览器（多用户同时使用不会冲突，`chatRWKV`暂不支持）
6. 对话历史管理（删除单条、清空）
7. 支持局域网、内网部署和多用户同时使用。
8. 多用户同时使用中会自动排队，并显示当前用户。

**欢迎同学们制作教学视频、懒人包等，做好请和我联系，我会把相关链接加到readme里**

**交流QQ群：162451840**
##  截图
#### 设置和预设功能
![](imgs/setting.png)
![](imgs/setting2.png)

## 懒人包
链接：https://pan.baidu.com/s/105nOsldGt5mEPoT2np1ZoA?pwd=lyqz 

提取码：lyqz

默认参数在GTX1660Ti（6G显存）上运行良好。
1. 旧版包含程序主体和chatGLM-6B、chatYuan，分别是独立的压缩文件。
2. chatRWKV模型更新频繁，请去官方链接下最新的。暂不支持chatPDF功能，很快就加上。
3. 新版暂时只有chatGLM-6B，但重新制作，体积更新，包含各种优化，集成知识库功能，推荐使用。
## 自行安装
### 1.安装库
通用依赖：```pip install -r requirements.txt```
知识库bing模式：```pip install -r requirements-bing.txt```
知识库fess模式：```pip install -r requirements-fess.txt```

### 2.下载模型
根据需要，下载对应模型。

建议使用chatRWKV的RWKV-4-Raven-7B-v7-ChnEng-20230404-ctx2048（截止4月6日效果较好），或chatGLM-6B。

### 3.参数设置
根据`settings.bat`中说明，填写你的模型下载位置等信息
### 4.生成知识库
将txt格式的语料放到txt文件夹中，运行`run_data_processing.bat`。
## 知识库
知识库最终效果是生成一些提示信息，会插入到对话里面。
fess模式、bing模式、bingxs模式、 bingsite模式均调用搜索引擎搜索获取答案。
搜索后在回答之前插入提示信息，知识库的数据就被模型知道了。
为防止爆显存，插入的数据不能太长，所以有字数限制。
知识库在线模式：```pip install -r requirements-bing.txt```
主要是有以下几种方案：
1.   bing模式，cn.bing搜索，仅国内可用
4.   bingxs模式，cn.bing学术搜索，仅国内可用
5.   bingsite模式，bing站内搜索，需设置网址
4.   fess模式，本地部署的[fess搜索](https://github.com/codelibs/fess)，效果好于已删除的s、x模式，并使用[letiantian/TextRank4ZH](https://github.com/letiantian/TextRank4ZH)进行了关键词提取
### win系统fess使用
1. 懒人包中下载fess-14.7.0-with-jdk.7z
2. 解压到平时放软件的盘
3. 打开解压出来的fess-14.7.0-with-jdk\bin目录
4. 双击fess.in.bat
5. 双击fess.bat. 弹出命令行运行框. 将其最小化
6. 打开浏览器. 打开网址http://localhost:8080/
7. 点击右上角log in  输入账号:admin 密码：wenda 进行登录
8. 点击侧边栏中的Crawler. 点击File System
9. 点击右上角的Create New
10. Name输入便于记忆的资料库的名字
11. Paths输入资料库的地址（格式示例：file:///E:/pdf）
12. 其余选项保持默认. 下滚至最下方点击Create
13. 自动返回File System页面. 点击刚才创建的选项（自己输入的Name）
14. 点击Create new job. 点击Create
15. 进入侧边栏的System内的Scheduler. 可以看到很多任务
16. 目录的前面可以看到刚刚创建的job（示例：File Crawler - pdf search）. 点击进入
17. 点击Start now. 刷新界面即可看到该任务正在运行. running
18. 此时fess就在爬取文件的名字和内容. 可以在资源管理器看到cpu有负载
19. 挂机。等待爬取完成即可尝试搜索关键词

####  调试工具
![](imgs/zsk-test.png)
####  chatGLM-6B模型
![](imgs/zsk-glm.png)


#### chatRWKV模型
![](imgs/zsk-rwkv.png)
### 1.索引语料
把自己的txt格式的文档放在名为txt的文件夹里，运行:
```run_data_processing.bat```
需要注意的是，索引语料至针对s、x模式，在线知识库（bing模式等）不需要索引，运行索引会直接报错。
### 2.使用
正常使用中，勾选右上角知识库
## chatGLM-6B
运行：`run_GLM6B.bat`。

模型位置等参数：修改`settings.bat`。

默认参数在GTX1660Ti（6G显存）上运行良好。

## chatRWKV
运行：`run_rwkv.bat`。

模型位置等参数：修改`settings.bat`。

默认参数在GTX1660Ti（6G显存）上正常运行，但速度较慢。

### 生成小说
![](imgs/novel.png)
#### 文字冒险游戏
![](imgs/wzmx.png)
## llama
运行：`run_llama.bat`。
模型位置等参数：修改`settings.bat`。
当前（4月12日）库对中文实现有bug，用plugins文件夹下的llama.py替换库中文件。
## chatYuan
YuanAPI.py

模型默认位置：ChatYuan-large-v2

这个最轻量，是电脑都能跑，但是智力差点
## TODO
实现以下知识库模组：
```
知识图谱
```
实现以下模型模组：
```
```
