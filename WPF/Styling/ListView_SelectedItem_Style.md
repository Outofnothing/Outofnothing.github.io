# 根据ListView中Item的选中状态修改其样式

## 首先分别定义选中状态和非选中状态下的DataTemplate

```xaml
<DataTemplate x:Key="PatientListViewItemTemplate" DataType="dm:Patient">
    <StackPanel Orientation="Horizontal">
        <Label
            Width="100"
            Margin="5,0"
            Content="{Binding Name}"
            Foreground="{StaticResource HighLightTextColor}"
            Style="{StaticResource MajorLabel}" />
        <Label
            Content="{Binding MedicalRecordId}"
            ContentStringFormat="病历号：{0}"
            Foreground="{StaticResource HighLightTextColor}"
            Style="{StaticResource SecondaryLabel}" />
    </StackPanel>
</DataTemplate>

<DataTemplate x:Key="SelectedPatientListViewItemTemplate" DataType="dm:Patient">
    <StackPanel Orientation="Horizontal">
        <Label
            Width="100"
            Margin="5,0"
            Content="{Binding Name}"
            Foreground="White"
            Style="{StaticResource MajorLabel}" />
        <Label
            Content="{Binding MedicalRecordId}"
            ContentStringFormat="病历号：{0}"
            Foreground="White"
            Style="{StaticResource SecondaryLabel}" />
    </StackPanel>
</DataTemplate>
```

## 然后，定义ContainerStyle，并根据选中状况使用不同的DataTemplate

```xaml
<Style x:Key="PatientListViewContainerStyle" TargetType="ListViewItem">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="ListViewItem">
                <Border
                    Name="_Border"
                    Margin="8,3"
                    Padding="4"
                    Background="#366698"
                    CornerRadius="8"
                    SnapsToDevicePixels="true">
                    <ContentPresenter />
                </Border>
                <ControlTemplate.Triggers>
                    <Trigger Property="IsSelected" Value="True">
                        <Setter TargetName="_Border" Property="Background" Value="#1cc0fd" />
                        <Setter Property="Foreground" Value="White" />
                    </Trigger>
                </ControlTemplate.Triggers>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
    <Setter Property="ContentTemplate" Value="{StaticResource PatientListViewItemTemplate}" />
    <Style.Triggers>
        <Trigger Property="IsSelected" Value="True">
            <Setter Property="ContentTemplate" Value="{StaticResource SelectedPatientListViewItemTemplate}" />
        </Trigger>
    </Style.Triggers>
</Style>
```