### Windows EFI分区误格式化恢复

使用大白菜，进入PE，打开命令行

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





