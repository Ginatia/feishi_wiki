# PySide6-1

## Designer

使用此工具用 Qt Designer 打开ui文件进行可视化操作

```pwsh
pyside6-designer.exe .\mainWindow.ui
```

## UIC

使用此工具将 ui文件 转为py文件，以供直接调用

```
pyside6-uic.exe .\mainWindow.ui -o test.py
```

## 基础框架

```python
from PySide6.QtCore import QSize,Qt
from PySide6.QtWidgets import QApplication,QMainWindow

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("MainWindow")
		self.setup_ui()

	def setup_ui(self)->None:

		pass


if __name__ == "__main__":
    app = QApplication([])
    window = MainWindow()
    window.show()

    app.exec()
```


## 信号与槽函数

click 信号连接到 btn_was_clicked 函数
按钮 btn 接收到 click信号，便会执行 `btn_was_clicked` 函数

```python
from PySide6.QtCore import QSize,Qt
from PySide6.QtWidgets import QApplication,QMainWindow,QPushButton

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("MainWindow")

        self.cnt = 0

        btn = QPushButton("Press Me!")
        btn.setCheckable(True)
        btn.clicked.connect(self.btn_was_clicked)

        self.setCentralWidget(btn)

    def btn_was_clicked(self):
        print(f"click {self.cnt}")
        self.cnt += 1


if __name__ == "__main__":
    app = QApplication([])
    window = MainWindow()
    window.show()

    app.exec()
```

## Signal，Slot

使用自定义信号连接槽函数
槽函数应使用 `@Slot` 进行装饰

```python
class TestObject(QObject):
    signal = Signal(str)

    def func(self):
        self.signal.emit("Hello")

@Slot(str)
def handle_signal(msg:str):
    print(f"receive {msg}")


if __name__ == "__main__":
    
    testObject = TestObject()
    testObject.signal.connect(handle_signal)
    testObject.func()
    
```


