# 第一个Qml程序

`import` 引入模块
声明 `Window` 对象，并对属性赋值

```qml
import QtQuick

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")
}

```

## 使用控件

首先在项目顶层 CMakeLists.txt 配置

```cmake
find_package(Qt6 REQUIRED COMPONENTS QuickControls2)
target_link_libraries(mytarget PRIVATE Qt6::QuickControls2)
```

ApplicationWindow 是 QtQuick.Controls 中预定义的类型

```qml
//import related modules
import QtQuick
import QtQuick.Controls

//window containing the application
ApplicationWindow {
    width: 640
    height: 480
    visible: true
    //title of the application
    title: qsTr("Hello World")

    //menu containing two menu items
    header: MenuBar {
        Menu {
            title: qsTr("&File")
            Action {
                text: qsTr("&Open...")
                onTriggered: console.log("Open action triggered")
            }
            MenuSeparator { }
            Action {
                text: qsTr("&Exit")
                onTriggered: Qt.quit()
            }
        }
    }

    //Content Area

    //a button in the middle of the content area
    Button {
        text: qsTr("Hello World")
        anchors.horizontalCenter: parent.horizontalCenter
        anchors.verticalCenter: parent.verticalCenter
    }
}

```

## 响应事件

`TapHandler` 是 QML 中用于处理触摸或鼠标点击事件的元素。

当用户在 `TapHandler` 所在的区域内点击或轻触时，它就会触发 **onTapped** 信号。

然后在 **onTapped** 编写代码响应事件，如改变属性的值。

```qml
import QtQuick

Window{
    id: root
    width: 200
    height: 200
    visible: true

    color: isRed ? "red" : "blue"

    property bool isRed: true

    Text{
        anchors.centerIn:parent
        text: "Hello QML"
    }

    TapHandler {
        onTapped: root.isRed = !root.isRed
    }
}

```

也可编写函数，触发信号时调用函数

```qml
import QtQuick

Window{
    id: root
    width: 200
    height: 200
    visible: true

    color: isRed ? "red" : "blue"

    property bool isRed: true

    Text{
        anchors.centerIn:parent
        text: "Hello QML"
    }

    function colorChange(){
        isRed = !isRed
        console.log(isRed)
    }

    TapHandler {
        onTapped: root.colorChange()
    }
}

```


