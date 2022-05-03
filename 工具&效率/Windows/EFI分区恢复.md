# [Windows EFI分区误格式化恢复](https://www.cnblogs.com/tonyc/p/8243807.html)

整Archlinux结果一个不小心把EFI分区格式化了，win10没法进，但是系统盘没动，到网上查了查恢复的方法，最终成功了，记录一下，免得下回再手残。

## 准备工作

首先需要一个U盘，win10的安装镜像和刻录软件，推荐UltalISO，刻好之后到启动菜单里面选择用U盘启动。

## 修复

启动之后会进入安装界面，第一步是选择语言的，直接点下一步。

点完之后可以直接选择“修复计算机”(左下角)，也可以选择“现在安装”，同意协议之后点下一步，选择“自定义：仅安装windows(高级)”，选中EFI分区(第二个)，删除，然后回退再回到之前选择修复。我是后面这种。现在不删等下到diskpart里面删除应该也一样。

点“修复计算机”之后，选择“疑难解答”，“命令提示符”，会出现命令提示符。

进入diskpart

```markdown
> diskpart
> list disk
> select disk 0//选择windows所在的那个
```

如果刚刚没删除efi分区需要删除一下

```sql
> list partition
> select partion 2
> delete partition
> select disk 0
```

创建efi分区并格式化

```shell
>create efi size=100//原来是100M大小的，所以有100M大小的空余
>select partition 2
>format quick
>assign letter=p
>exit
```

恢复

```shell
> cd c:\windows\system32//盘符可以bootrec /sacnos查查
> c:
>bcdboot c:\windows /s p: /f UEFI /l zh-cn //p是刚刚指定给efi分区的盘符。成功的话会提示复制成功
> exit
```

重启