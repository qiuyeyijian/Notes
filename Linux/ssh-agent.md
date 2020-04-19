如果您使用随 GitHub Desktop 一起安装的 Git Shell，则*不*需要执行这些步骤。 GitHub Desktop 会为您自动启动 `ssh-agent`。

否则，打开 bash 或 Git shell 时执行这些步骤以自动运行 `ssh-agent`。 复制以下行并将其粘贴到 Git shell 中的 `~/.profile` 或 `~/.bashrc` 文件中：

```bash
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

If your private key is not stored in one of the default locations (like `~/.ssh/id_rsa`), you'll need to tell your SSH authentication agent where to find it. 要将密钥添加到 ssh-agent，请输入 `ssh-add ~/path/to/my_key`。



> **提示：**如果想要 `ssh-agent` 在一段时间后忘记您的密钥，可通过运行 `ssh-add -t <seconds> ` 进行配置。