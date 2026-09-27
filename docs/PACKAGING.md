# TreeMind Windows 安装包

## 给使用者

运行 `TreeMind-Setup-1.0.0-x64.exe`，按中文向导安装。默认安装在当前用户目录，不要求管理员权限；桌面和开始菜单提供 TreeMind 入口，安装结束可直接启动。

目标平台为 Windows 10（2004及以上）/11 x64。安装包包含 Python、Qt、网页导图组件等运行依赖，用户无需安装 Python 或配置环境。AI 功能仍需要使用者自己的模型账号和 API Key；普通导图编辑可以离线运行。

此构建未配置代码签名证书，Windows 下载或启动时可能提示发布者未知。安装包附带 SHA-256 校验文件。不要将 SHA-256 校验等同于发布者数字签名。

可从 Windows「已安装的应用」或开始菜单卸载。卸载不主动删除个人导图、应用设置或 Windows 凭据中的密钥。

## 给维护者

源码仍可直接运行，打包配置集中在 `packaging/`，不包含个人导图目录、`.venv`、用户设置或密钥。安装目录的 `source/TreeMind-source.zip` 保留对应项目源码；第三方声明和依赖许可证位于 `_internal` 中。

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-build.txt
.\.venv\Scripts\python.exe scripts/build_release.py --iscc "C:\路径\Inno Setup 6\ISCC.exe"
```

构建环境使用 PyInstaller 6.22.3、Inno Setup 6.7.3。当前电脑的编译器位于 `build/tools/InnoSetup/ISCC.exe`，运行脚本不传 `--iscc` 也可使用。安装包输出到 `dist/installer/`。

PyInstaller 先生成目录式程序 `dist/TreeMind/`，再由 Inno Setup 压缩为单个安装 EXE。不要只分发目录中的 `TreeMind.exe`，它依赖同目录的 `_internal`。升级时保持 `.iss` 中 AppId 不变，并同步修改版本和构建脚本中的文件名。

可执行程序支持离线自检，测试使用临时设置与示例导图，不调用 AI：

```powershell
& ".\dist\TreeMind\TreeMind.exe" --self-test "C:\可写目录\TreeMind-check.json"
```

检查报告包含编辑器初始化、节点编辑与撤销、图片随 JSON 保存重开、功能模块加载。该检查不能代替所有 Windows 版本和真实模型账号测试。

打包本身不改变项目或第三方组件的许可。当前 PyQt 安装来自开源发行包；对外分发和商业发布前，请按项目实际采用的许可处理对应源码与第三方授权。参见 [第三方声明](../THIRD_PARTY_NOTICES.md)。

构建工具参考：[PyInstaller 使用说明](https://www.pyinstaller.org/en/stable/usage.html)、[Inno Setup](https://jrsoftware.org/isdl.php)。简体中文翻译来自 Inno Setup 官方源码仓库，文件保留了原维护者说明。
