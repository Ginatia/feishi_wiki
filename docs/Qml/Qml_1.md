# 创建由C++类定义的自定义QML类型

## 选择 QQuickPaintedItem 作为基类

确保有 `Q_OBJECT` 和 `QML_ELEMENT` 宏

```cpp
#ifndef CIRCLE_H
#define CIRCLE_H

#include <QColor>
#include <QPainter>
#include <QQuickPaintedItem>

class Circle : public QQuickPaintedItem
{
    Q_OBJECT
    QML_ELEMENT
public:
    Circle();
public slots:

signals:

private:
};

#endif // CIRCLE_H


```

## 添加属性

为圆添加半径和颜色共两个属性。
每个属性有 get，set，以及属性改变时触发的信号函数。

```cpp
class Circle : public QQuickPaintedItem
{
    Q_OBJECT
    QML_ELEMENT

    Q_PROPERTY(double radius READ radius WRITE setRadius NOTIFY radiusChanged FINAL)
    Q_PROPERTY(QColor color READ color WRITE setColor NOTIFY colorChanged FINAL)

public:
    Circle();

    double radius()const;
    void setRadius(double nRadius);

    QColor color()const;
    void setColor(QColor nColor);

signals:
    void radiusChanged();
    void colorChanged();

private:
    double m_radius;
    QColor m_color;
};
```

## 定义QML可调用函数

默认普通的类成员函数是不暴露的。
需要添加 `Q_INVOKABLE` 宏。

```cpp
Q_INVOKABLE void setProperties(double nRadius,QColor nColor);


void Circle::setProperties(double nRadius, QColor nColor)
{
    setRadius(nRadius);
    setColor(nColor);
}
```

## 添加槽函数以获取属性信息


```cpp
public slots:
    void circleInfo();
    void onPropertyChanged();
```

```cpp
void Circle::circleInfo()
{
    qInfo() << QString("Circle color: %1 - Circle radius: %2")
    .arg(m_color.name()).arg(m_radius);
}

void Circle::onPropertyChanged()
{
    update();
}
```

## 将信号连接到槽函数


```cpp
Circle::Circle()
{
    connect(this,&Circle::radiusChanged,this,&Circle::onPropertyChanged);
    connect(this,&Circle::colorChanged,this,&Circle::onPropertyChanged);
}
```

## paint

重写 paint 函数，以实现自定义渲染。

```cpp
void paint(QPainter *painter)override;

void Circle::paint(QPainter *painter)
{
    QPen pen(m_color,m_radius);
    painter->setPen(pen);
    painter->setRenderHints(QPainter::Antialiasing, true);
    painter->drawEllipse(
        QRect(m_radius / 2, m_radius / 2,
        width() - m_radius,
        height() - m_radius)
    );
}
```


