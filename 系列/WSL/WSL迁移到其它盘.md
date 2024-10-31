### 1. 整体流程
```powershell
wsl --shutdown # 首先停止正在运行的Linux子系统
wsl -l -v # 查看本机中的Linux子系统列表
wsl --export Ubuntu-20.04 E:\wsl\ubuntu_20_04.tar # Ubuntu-20.04是Linux子系统的Name，后面的.tar则是导出的文件名
wsl --unregister Ubuntu-20.04 # 在Linux子系统列表中卸载这个子系统
wsl --import ubuntu20_04 E:\wsl E:\wsl\ubuntu_20_04.tar --version 2 # 导入子系统

```


