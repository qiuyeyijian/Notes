

### 定时创建文件



```bash
#! /bin/bash
i=1
while [ $i ]; do
        echo "create a file"
        touch "$i.txt"
        sleep 1s
        ((i++))
done
```

