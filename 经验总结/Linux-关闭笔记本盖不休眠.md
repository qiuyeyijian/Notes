# Linux-关闭笔记本盖不休眠

2017年04月17日 17:59:25 [清无时空](https://me.csdn.net/sinat_34439107) 阅读数：6663



 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/sinat_34439107/article/details/70214289

systemd 处理某些电源相关的 ACPI事件，可以通过从 /etc/system/logind.conf以下选项进行配置：

> HandlePowerKey按下电源键后的行为，默认power off
>
> HandleSleepKey 按下挂起键后的行为，默认suspend
>
> HandleHibernateKey 按下休眠键后的行为，默认hibernate
>
> HandleLidSwitch 合上笔记本盖后的行为，默认suspend

触发的行为可以有

> ignore、power off、reboot、halt、suspend、hibernate、hybrid-sleep、lock 或 exec。

如果要合盖不休眠只需要把HandleLidSwitch选项设置为如下即可：

```
HandleLidSwitch=lock
```

注意：设置完成保存后运行下列命令才生效。

```
systemctl restart systemd-logind
```