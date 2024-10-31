### 1. 整体流程
```powershell
wsl --shutdown # 首先停止正在运行的Linux子系统
wsl -l -v # 查看本机中的Linux子系统列表
wsl --export Ubuntu-20.04 E:\wsl\ubuntu_20_04.tar # Ubuntu-20.04是Linux子系统的Name，后面的.tar则是导出的文件名
wsl --unregister Ubuntu-20.04 # 在Linux子系统列表中卸载这个子系统
wsl --import ubuntu20_04 E:\wsl E:\wsl\ubuntu_20_04.tar --version 2 # 导入子系统
wsl -d ubuntu20_04 -u yrx --cd ~ # 使用该命令指定分发版、用户、路径启动子系统
```
### 2. 创建启动链接
	要进入我们刚刚创建的分发版本中，需要运行 `wsl -d ubuntu20_04 -u yrx --cd ~`，但每次要进入都需要先打开 cmd 再运行显然太慢，我们通过 windows 支持的 `lnk` 方式（快捷方式）来简化操作。
	直接右键选择新建 `快捷方式`，将以下内容

>C:\Users\42162\AppData\Local\Microsoft\WindowsApps\wsl. exe -d ubuntu 20_04 -u yrx --cd ~

	粘贴进去，顺着操作就好了，命令中的参数要替换成自己的。



