# Visual Studio Files are not compiled，文件不编译

当将一个文件从一个项目移到迁移到当前项目时，可能触发新项目中文件不编译的bug

查看新项目的csproj文件可以看到以下行

```xml
<ItemGroup>
    <Compile Remove="example.cs" />
</ItemGroup>
```

类似的问题，可以参见[这里](https://developercommunity.visualstudio.com/t/files-being-added-to-itemgroupcompileremove/308286)