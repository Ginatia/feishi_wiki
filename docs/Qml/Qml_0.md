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


## 自定义信号

在 SquareButton 中声明两个信号。

在 MouseArea 中发出信号。

在实例中添加信号处理程序。

```qml

import QtQuick

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    component SquareButton: Rectangle{
        id: root

        signal activated(xPosition:real,yPosition:real)
        signal deactivated

        property int side: 100
        width: side
        height: side
        color: "#2CDE85"

        MouseArea{
            anchors.fill: parent

            onReleased: root.deactivated()

            onPressed: (mouse) => root.activated(mouse.x,mouse.y)
        }
    }

    SquareButton {
        onDeactivated: console.log("Deactivated")

        onActivated: (xPos,yPos) => console.log("Activated at: ",xPos,yPos)
    }
}

```

## 对象方法

可以调用它来执行某些处理或触发后续事件。

方法可以连接到信号 ，以便在发出信号时自动调用，也可以通过属性绑定进行调用。 

可以调用定义方法的对象中定义的方法，也可以从它所暴露的另一个对象中调用它。

Timer 调用 rect 中的 colorRandom 函数，实现了按周期改变 rect 的颜色。


```qml
import QtQuick

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Rectangle{
        id: rect
        visible: true
        color: "red"
        function calcHeight() : real {
            return rect.width / 2
        }

        function colorRandom(){
            const r = Math.random();
            const g = Math.random();
            const b = Math.random();
            rect.color = Qt.rgba(r,g,b,1);
        }

        width: 100
        height: calcHeight()

        Timer {
            interval: 1000
            running: true
            repeat: true
            onTriggered: rect.colorRandom()
        }
    }
}
```

## 组件类型

组件类型封装了一段可重用的 QML 代码块，使其成为构建应用程序结构的重要基石。

组件通常被定义为 QML 文件 (.qml) 中的独立 QML 文档。

组件类型允许将 QML 组件直接定义在 QML 文档中，而不是单独的 .qml 文件中。

这对于在 QML 文件中重用小型组件，或者定义一个逻辑上与其他 QML 组件关联的组件非常有用。

由于组件并非派生自项 (Item)，因此无法将任何内容锚定到它。此外，它只能包含一个顶级项。

```qml

import QtQuick


Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Component {
        id : redSquare

        Rectangle{
            width: 100
            height: 100
            color: "red"
            border.color: "black"
        }
    }

    Column{
        anchors.centerIn: parent

        Repeater{
            model: 3
            delegate: redSquare
        }
    }
}


```

## Repeater 中继器

Repeater 是 QML 中一个强大的元素，它允许基于数据模型生成同一项目的多个实例。

Repeater 无需手动定义每个项目，而是根据需要动态创建，从而节省时间并简化大量项目的管理。

```qml

import QtQuick


Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Component {
        id : number

        Text{
            required property int index

            text: `${index}`
            color: "black"
            font.pixelSize: 80
        }
    }

    Column{
        anchors.centerIn: parent

        Repeater{
            model: 3
            delegate: number
        }
    }
}


```


## 附属属性

QML 还允许在无需项目本身知晓的情况下，为项目“附加”额外的属性。

这些属性被称为附加属性 。与常规属性不同，附加属性主要供其他与项目交互的项目使用，以便它们稍后读取或写入这些属性。

附加属性扩展了项目的功能，使其能够满足特定的使用场景，而无需修改项目本身。

```qml
import QtQuick


Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")

    Component {
        id : square

        Rectangle{
            width: 100
            height: 100
            color: Positioner.isFirstItem ? "green" : "red"
            border.color: "black"
        }
    }

    Column{
        anchors.centerIn: parent

        Repeater{
            model: 3
            delegate: square
        }
    }
}

```