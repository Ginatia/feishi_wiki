# Qml 最佳实践

## 使用强类型

使用 size 类型属性，具有宽度和高度的结构值。

相较于 var 定义的变量更具强类型，因为var接受任何值。

```qml
import QtQuick
import QtQuick.Layouts

Window{
    id: window
    visible: true
    property size size: ({width:1280,height:800})

    width: size.width
    height: size.height

    RowLayout{
        id: mainLayout
        anchors.centerIn: parent
        spacing: 20

        Rectangle{
            id: rect_1
            visible: true
            property size size: ({width:400,height:400})

            width: size.width
            height: size.height
            color: "#00fa9a"

            anchors.left: window.left
        }

        Rectangle{
            id: rect_2
            visible: true
            property var size: Qt.size(400,400)

            width: size.width
            height: size.height
            color: "#ff6347"

            anchors.right: window.right
        }
    }

}


```


## 避免鸭子类型

鸭子类型是指在假设对象支持某些方法或属性。

在 updatetext 函数中指明参数类型，严格遵循类型安全。

若不指定类型，则会因为 Rectangle 没有此属性而报错。


```qml
import QtQuick
import QtQuick.Layouts
import QtQuick.Controls

Window{
    id: window
    visible: true
    property size size: ({width:400,height:200})

    width: size.width
    height: size.height

    RowLayout{
        id: mainLayout

        function updatetext(item:Label){
            if(item){
                item.text = `Updated`;
            }
        }

        Label{
            text: `Label`
        }

        Text{
            text: `Label`
        }

        Rectangle{
            width: 100
            height: 100
            color: "#00fa9a"
        }
        Component.onCompleted: {
            for(let i = 0; i < children.length;i++){
                updatetext(children[i]);
            }
        }
    }

}



```

## 使用声明式绑定

在第一个矩形中， 宽度属性声明性绑定在滑块值上。这意味着每当滑块值变时宽度会自动更新，确保动态且无缝的关系。声明式绑定是 QML 反应式编程模型的核心，使代码简洁、可维护且可预测。

在第二个矩形中，Component.onCompleted 处理器中使用命令式赋值，根据 slider.value 设置宽度。与声明式绑定不同，这种方法只在组件初始化时赋予一次值。随后对滑块值的更改不会更新矩形的宽度 ，从而破坏预期的反应行为。

```qml
import QtQuick
import QtQuick.Layouts
import QtQuick.Controls

Window{
    id: window
    visible: true
    property size size: ({width:400,height:200})

    width: size.width
    height: size.height

    ColumnLayout {
        id: mainLayout

        Row{
            anchors.fill: parent
            Rectangle{
                width: slider.value
                height: 50
                color: "#00fa9a"
            }
            Rectangle{
                width: 100
                height: 50
                color: "#ff6347"

                Component.onCompleted: {
                    width = slider.value
                }
            }
        }
        Slider{
            id: slider

            from: 50
            to: 125
            value: 50
        }
    }

}
```

## 不依赖属性评估与信号发射

虽然 Timer 在 Label 之后定义，

但实际的执行是由 QML 依赖解析完成的。

设计组件时不能依赖特定的执行顺序。

```qml
import QtQuick
import QtQuick.Layouts
import QtQuick.Controls

Window{
    id: root
    visible: true
    property size size: ({width:400,height:200})

    property int rootValue: 0

    width: size.width
    height: size.height

    ColumnLayout {
        id: mainLayout

        Label{
            id: label_1
            text: root.rootValue

            onTextChanged: {
                console.log(`Label 1 ${root.rootValue}`);
            }
        }

        Label{
            id: label_2
            text: root.rootValue

            onTextChanged: {
                console.log(`Label 2 ${root.rootValue}`);
            }
        }
    }

    Timer{
        interval: 1000
        running: true
        repeat: true

        onTriggered: {
            root.rootValue++;
        }
    }
}
```


## 使用类型列表属性

使用 var 存放任意变量，但是无法定位存放了不同类型的 button 。

而使用 `list<CustomButton>` 则会报错类型不一致。

```qml
import QtQuick
import QtQuick.Layouts
import QtQuick.Controls

Window{
    id: root
    visible: true
    property size size: ({width:400,height:200})

    property int rootValue: 0

    width: size.width
    height: size.height

    component CustomButton : Button{
        text: "Custom Button"
    }

    ColumnLayout {
        id: colLayout

        CustomButton{
            id: cb1
        }
        CustomButton{
            id: cb2
        }
        CustomButton{
            id: cb3
        }

        Button{
            id:sb
            text: "Standard Button"
        }

        property var buttons: [cb1,cb2,cb3,sb]
        property list<CustomButton> cbs: [cb1,cb2,cb3,sb]
    }

}
```

## 使用 required 修饰属性

在这个例子中，Label 代理在中继器中声明三个必需属性： 名称 、 价格和颜色 。

每当创建代理实例时，这些属性必须显式设置。

如果缺少或错误分配了这些属性，QML 引擎会在运行时触发错误，确保代理的期望得到满足。


```qml
import QtQuick
import QtQuick.Layouts
import QtQuick.Controls

Window{
    id: root
    visible: true
    property size size: ({width:400,height:200})

    property int rootValue: 0

    width: size.width
    height: size.height

    ColumnLayout{
        anchors.fill: parent

        Repeater{
            model: [
                {"name":"N1","price":10,"color":"darkslategray"},
                {"name":"N2","price":20,"color":"darkcyan"},
                {"name":"N3","price":30,"color":"darkseagreen"}
            ]

            delegate: Label{
                required property string name
                required property real price
                required color
                text: `${name} - ${price} CNY`
            }
        }
    }
}
```