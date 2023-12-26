# 为TextBox创建一个ControlTemplate

## 为TextBox的四周添加圆角
首先，看一个简单的场景：为TextBox的四周添加圆角。

那么，我们首先定义一个名为`SimpleRoundedTextBox`的`ControlTemplate`，并添加一个Border
```xaml
 <ControlTemplate x:Key="SimpleRoundedTextBox" TargetType="TextBox">
     <Border Background="Blue" CornerRadius="10" />
 </ControlTemplate>
```

然后，将TextBox的Template设为`SimpleRoundedTextBox`。

```xaml
 <TextBox
    Width="200"
    Height="40"
    Template="{StaticResource SimpleRoundedTextBox}" />
```

可是，现在我们根本无法在TextBox中输入任何字符，TextBox与一个Border没有差别。

为了能够输入字符，我们需要做的是在Border下添加一个ScrollViewer，并且把它的名字设为**PART_ContentHost**

```xaml
<TextBox>
    <TextBox.Template>
        <ControlTemplate TargetType="TextBox">
            <Border>
                <ScrollViewer Name="PART_ContentHost" />
            </Border>
        </ControlTemplate>
    </TextBox.Template>
</TextBox>
```
现在，我们可以正常输入字符了。

## ScrollViewer的作用是什么呢，为什么名字一定要是**PART_ContentHost**?

首先，WPF需要知道如何将用户的输入绑定到你自定义的ControlTemplate，这就要求我们显示地指定ControlTemplate中的一个元素，用于绑定。我们暂且将这个元素叫做Host好了。

那么，我们怎么指定这个元素呢？这需要把这个元素的名称设为PART_ContentHost即可。

这时因为TextBox的[源码](https://github.com/dotnet/wpf/blob/main/src/Microsoft.DotNet.Wpf/src/PresentationFramework/System/Windows/Controls/Primitives/TextBoxBase.cs#L1974)中有这么一段：

```cs
_textBoxContentHost = GetTemplateChild("PART_ContentHost") as FrameworkElement;
```

可以看到，TextBox的源码会按照PART_ContentHost查找作为绑定Host用的`FrameworkElement`。找到这个`FrameworkElement`后，WPF便会将需要绘制的内容绑定到它上面。

