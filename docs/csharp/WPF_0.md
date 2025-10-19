## 项目简述

一个最简单的WPF项目由**xaml文件**以及**cs文件**构成。
xaml文件是基于xml的标记语言，用于声明式地定义用户界面。
cs文件则负责逻辑处理和事件响应。

## XAML

xaml的标签既可以自闭合也可以由开始结束标签组成。
但是自闭合没有子元素。

```xaml
<Window x:Class="WpfApp1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp1"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <Button>
            <Button.FontWeight>Bold</Button.FontWeight>
            <Button.Content>
                <WrapPanel>
                    <TextBlock Foreground="Blue">Multi</TextBlock>
                    <TextBlock Foreground="Red">Color</TextBlock>
                    <TextBlock>Button</TextBlock>
                </WrapPanel>
            </Button.Content>
        </Button>
    </Grid>
</Window>
```

## CSharp

这里的 `partial` 意为部分类。
将自动生成的代码与手写的代码分别在多个文件中隔离。
这些文件在编译时会被**自动合并成一个完整的类型**。

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }
}
```

## 事件

在xaml文件中为按钮声明点击事件。
Click 是点击事件。
Button_Click 是事件触发时的回调函数。

```csharp
<Button Click="Button_Click">
    <Button.Width>400</Button.Width>
    <Button.Height>200</Button.Height>
    <Button.FontWeight>Bold</Button.FontWeight>
    <Button.Content>
        <WrapPanel>
            <TextBlock Foreground="Blue">Multi</TextBlock>
            <TextBlock Foreground="Red">Color</TextBlock>
            <TextBlock>Button</TextBlock>
        </WrapPanel>
    </Button.Content>
</Button>
```

在对应的cs文件中会生成对于的函数定义。

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    MessageBox.Show("Button clicked!");
}
```

并且有IDE自动生成的代码

```csharp
((System.Windows.Controls.Button)(target)).Click += new System.Windows.RoutedEventHandler(this.Button_Click);
```

