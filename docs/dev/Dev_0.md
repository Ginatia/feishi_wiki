# 开发环境

## 包管理器 Scoop

Scoop 是Windows的命令行安装程序

Scoop 可以通过命令行安装程序

防止安装大量程序造成 PATH 污染

避免安装和卸载程序时产生意外的副作用

自动查找并安装依赖项

自行执行所有额外的设置步骤以获得可运行的程序

### 安装Scoop

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

### 使用Scoop 安装程序

```powershell
scoop bucket add extras
scoop install extras/vscode
```

---

## Python包和项目管理器 uv

### 安装 uv

```powershell
scoop bucket add main
scoop install main/uv
```

### 使用uv安装 Python

安装最新版本的Python

```powershell
uv python install
```

安装指定版本的Python

```powershell
uv python install 3.12
```

查看可用与已安装的Python

```powershell
uv python list
```

创建虚拟环境

```powershell
uv venv venv_name
.\venv_name\Scripts\activate
```

下载第三方库

```powershell
uv pip install numpy
```


执行python文件

```
uv run main.py
python main.py
```




