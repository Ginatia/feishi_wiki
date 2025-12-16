# 定位器

## Row

将子项从左至右摆放。

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    Row {
        anchors.centerIn: parent

        Repeater{
            model: 3
            delegate: square
        }
    }
}

```

## Column

按列从上至下摆放子项

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
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

## Grid

Grid 组件以二维网格形式排列项目， 行和列数均可配置。

默认情况下， 列数为 4。如果网格中的项目不足以填充指定的列数，则某些列的宽度将为零。


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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    Grid {
        anchors.centerIn: parent

        columns: 2
        spacing: 10

        Repeater{
            model: 4
            delegate: square
        }
    }
}

```


## Flow

Flow 的工作方式与 Grid 类似，但在处理可变大小的内容时更加灵活。

项目并排排列，当它们到达可用空间的末尾时，会自动换行，类似于段落中的文本换行。

这对于响应式布局尤其有用，因为内容必须根据窗口或屏幕尺寸动态调整。

Flow 包含与 Grid 类似的属性，但只有一个间距属性来控制项目之间的间距。

请注意， Flow 枚举使用 Flow 而不是 Grid 。例如， Flow.TopToBottom 。

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    Flow {
        anchors.fill: parent
        spacing: 10

        Repeater{
            model: 10
            delegate: square
        }
    }
}

```

# 布局

## Flickable

Flickable QML 类型允许创建可“滑动”或滚动的视图，非常适合处理超出可见区域的大量内容。

Flickable 允许用户通过拖动或滑动来交互式地滚动浏览内容，从而创建支持水平和垂直移动的自然触控界面。

```qml
Flickable {
    anchors.fill: parent

    contentWidth: image.width
    contentHeight: image.height

    Image {
        id: image
        source: "assets/image0.jpg"
    }
}
```

## RowLayout

RowLayout QML 类型将其子项水平排列成单行，类似于 Row Positioner ，但它在使用 Layout 附加属性时，可以更好地控制项的大小和对齐方式。

它支持诸如 spacing 和 layoutDirection 之类的属性，用于控制项之间的间距以及它们的排列方向（从左到右或从右到左）。

```qml
import QtQuick
import QtQuick.Layouts

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    RowLayout {
        anchors.fill: parent

        Repeater{
            model: 3
            delegate: square
        }
    }
}

```

## ColumnLayout

ColumnLayout QML 类型与 RowLayout 类似，但它将项目垂直排列在一列中。

它适用于创建垂直列表或表单布局，在这些布局中，元素需要彼此堆叠。

ColumnLayout 也包含相同的 spacing 和 layoutDirection 属性，用于管理项目之间的距离和排列方向。

请记住再次使用 Layout 附加属性来修改大小、对齐方式和边距。

```qml
import QtQuick
import QtQuick.Layouts

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    ColumnLayout {
        anchors.fill: parent

        Repeater{
            model: 3
            delegate: square
        }
    }
}

```

## GridLayout

GridLayout 布局以二维网格形式排列项目，行和列数均可配置。

它尤其适用于需要双向排列项目的复杂布局，例如图片库。GridLayout 提供诸如 rowSpacing 、 columnSpacing 、 layoutDirection 和 flow 等属性，用于控制项目的间距和方向，以及优先填充行或列的优先级。

还可以指定网格的行数或列数 。

```qml
import QtQuick
import QtQuick.Layouts

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
            color: "red"
            border.color: "black"

            required property int index

            Text {
                id: mText
                text: qsTr(`text ${index}`)
            }
        }
    }

    GridLayout {
        anchors.fill: parent

        columns: 3

        Repeater{
            model: 9
            delegate: square
        }
    }
}


```