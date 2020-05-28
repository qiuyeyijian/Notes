







Win10 IIS默认是.net 4.0，安装VS2015后，IIS没有.net 4.5，解决方法，直接在CMD命令行下执行：

```
dism /online /enable-feature /featurename:IIS-ISAPIFilter
dism /online /enable-feature /featurename:IIS-ISAPIExtensions
dism /online /enable-feature /featurename:IIS-NetFxExtensibility45
dism /online /enable-feature /featurename:IIS-ASPNET45
```





