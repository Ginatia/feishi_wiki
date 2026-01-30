# 多个页面

## XAML 跳转

可以设定 `Hyperlink` 下的属性 `NavigateUri` 为下一个页面。

```xaml

<Hyperlink NavigateUri="page_2.xaml">
	Go To The Next Page
</Hyperlink>

```

## C Sharp 跳转

在此设定点击按钮，响应事件为导航为下一个页面。

```csharp

using System.Windows.Navigation;

private void Button_Click(object sender,RoutedEventArgs e)
{
	NavigationService.Navigate(
		new Uri("/page_2.xaml",UriKink.Relative)
	);
}

```

## 数据绑定

