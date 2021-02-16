## 计算机二级python试题练习

### 1、天龙八部

《天龙八部》是著名作家金庸的代表作之一，历时4年创作完成。该作品气势磅礴，人物众多，非常经典。这里给出一个《天龙八部》的网络版本，文件名为“天龙八部-网络版.txt”。

#### 问题一：

请编写程序，对这个《天龙八部》文本中出现的汉字和标点符号进行统计，字符与出现次数之间用冒号:分隔，输出保存到“天龙八部-汉字统计.txt”文件中，该文件要求采用 txt格式存储，参考格式如下（注意，不统计空格和回车字符）：

> ```
> 天:100, 龙:110, 八:109, 部:10
> ```

#### 参考代码：

``` python
fi = open("天龙八部-网络版.txt", "r", encoding='utf-8')
fo = open("天龙八部-汉字统计.txt", "w", encoding='utf-8')
txt = fi.read()
d = {}
for c in txt:
    d[c] = d.get(c, 0) + 1
del d[' ']
del d['\n']
ls = []
for key in d:
    ls.append("{}:{}".format(key, d[key]))
fo.write(",".join(ls))
fi.close()
fo.close()
```



#### 问题二：

请编写程序，对《天龙八部》文本中出现的中文词语进行统计，采用 `jieba` 库分词，词语与出现次数之间用冒号:分隔，输出保存到“天龙八部-词语统计.txt”文件中。参考格式如下（注意，不统计任何标点符号）：

> ```
> 天龙:100, 八部:10
> ```

#### 参考代码：

``` python
import jieba
fi = open("天龙八部-网络版.txt", "r", encoding='utf-8')
fo = open("天龙八部-词语统计.txt", "w", encoding='utf-8')
txt = fi.read()
words = jieba.lcut(txt)
d = {}
for w in words:
    d[w] = d.get(w, 0) + 1
del d[' ']
del d['\n']
ls = []
for key in d:
    ls.append("{}:{}".format(key, d[key]))
fo.write(",".join(ls))
fi.close()
fo.close()
```



### 2、论语

《论语》是儒家学派的经典著作之一，主要记录了孔子及其弟子的言行。网络上有很多《论语》文本版本。这里给出了一个版本，文件名称为“论语-网络版.txt”，其内容采用如下格式组织：

```
【原文】
 
1.11子曰：“父在，观其(1)志；父没，观其行(2)；三年(3)无改于父之道(4)，可谓孝矣。”
 
【注释】
 
（略）
 
【译文】
 
（略）
 
【评析】
 
（略）
```

该版本通过【原文】标记《论语》原文内容，采用【注释】、【译文】和【评析】标记对原文的注释、译文和评析。

#### 问题一：

请编写程序，提取《论语》文档中所有原文内容，输出保存到“论语-提取版.txt”文件。输出文件格式要求：去掉文章中原文部分每行行首空格及如“1.11”等的数字标志，行尾无空格、无空行。参考格式如下（原文中括号及内部数字是对应源文件中注释项的标记）：

```
子曰(1)：“学(2)而时习(3)之，不亦说(4)乎？有朋(5)自远方来，不亦乐(6)乎？人不知(7)，而不愠(8)，不亦君子(9)乎？”
 
有子(1)曰：“其为人也孝弟(2)，而好犯上者(3)，鲜(4)矣；不好犯上，而好作乱者，未之有也(5)。君子务本(6)，本立而道生(7)。孝弟也者，其为人之本与(8)？”
 
子曰：“巧言令色(1)，鲜(2)仁矣。”
 
（略）
```

#### 参考代码：

``` python
fi = open("论语-网络版.txt", "r", encoding='utf-8')
fo = open("论语-提取版.txt", "w")
wflag = False
for line in fi:
    if "【" in line:
        wflag = False
    if "【原文】" in line:
        wflag = True
        continue
    if wflag == True:
        for i in range(0,25):
            for j in range(0,25):
                line = line.replace("{}·{}".format(i,j), "**")
        for i in range(0,10):
            line = line.replace("*{}".format(i), "")
        for i in range(0,10):
            line = line.replace("{}*".format(i),"")
        line = line.replace("*","")
        fo.write(line)

fi.close()
fo.close()
```



#### 问题二：

请编写程序，在“论语-提取版.txt”基础上，进一步去掉每行文字中所有括号及其内部数字，保存为“论文-原文.txt”文件。参考格式如下： 

```
子曰：“学而时习之，不亦说乎？有朋自远方来，不亦乐乎？人不知，而不愠，不亦君子乎？”
 
有子曰：“其为人也孝弟，而好犯上者，鲜矣；不好犯上，而好作乱者，未之有也。君子务本，本立而道生。孝弟也者，其为人之本与？”
 
子曰：巧言令色，鲜仁矣。”
 
（略）
```

#### 参考代码：

``` python
fi = open("论语-提取版.txt", "r")
fo = open("论语-原文.txt", "w")
for line in fi:   #逐行遍历
    for i in range(1,23):  #对产生1到22数字 
        line = line.replace("({})".format(i), "")  #构造(i)并替换
    fo.write(line)
fi.close()
fo.close()
```

### 3、命运&寻梦

《命运》和《寻梦》都是著名科幻作家倪匡的科幻作品。这里给出一个《命运》和《寻梦》的网络版本，文件名为“命运-网络版.txt”和“寻梦-网络版.txt”。

#### 问题一：

请编写程序，对这两个文本中出现的字符进行统计，字符与出现次数之间用冒号:分隔，将两个文件前 100 个最常用字符分别输出保存到“命运-字符统计.txt”和“寻梦-字符统计.txt”文件中，该文件要求采用 CSV 格式存储，参考格式如下（注意，不统计回车字符）：

> 命:90, 运:80, 寻:70, 梦:60
>
> （略）

#### 参考代码

``` python
names = ["命运", "寻梦"]
for name in names:
    fi = open(name+"-网络版.txt", "r", encoding="utf-8")
    fo = open(name+"-字符统计.txt", "w", encoding="utf-8")
    txt = fi.read()
    d = {}
    for c in txt:
        d[c] = d.get(c, 0) + 1
    del d['\n']
    ls = list(d.items())
    ls.sort(key=lambda x:x[1], reverse=True)
    for i in range(100):
        ls[i] = "{}:{}".format(ls[i][0], ls[i][1])
    fo.write(",".join((ls[:100])))
    fi.close()
    fo.close()

```



#### 问题二：

请编写程序，对“命运-字符统计.txt”和“寻梦-字符统计.txt”中出现的相同字符打印输出。“相同字符.txt”文件中，字符间使用逗号分隔。

#### 参考代码

``` python
def getList(name):
    f = open(name+"-字符统计.txt", "r", encoding="utf-8")
    words = f.read().split(',')
    for i in range(len(words)):
        words[i] = words[i].split(':')[0]
    f.close()
    return words


def main():
    fo = open("相同字符.txt", "w")
    ls1 = getList("命运")
    ls2 = getList("寻梦")
    ls3 = []
    for c in ls1:
        if c in ls2:
            ls3.append(c)
    fo.write(",".join(ls3))
    fo.close()


main()

```

### 4、十二星座

古代航海人为了方便在航海时辨别方位和观测天象，将散布在天上的星星运用想象力将它们连接起来，有一半是在古时候已命名，另一半是近代开始命名的。两千多年前古希腊的天文学家希巴克斯命名十二星座，依次为白羊座、金牛座、双子座、巨蟹座、狮子座、处女座、天秤座、天蝎座、射手座、魔蝎座、水瓶座和双鱼座。给出二维数据存储CSV文件（SunSign.csv），内容如下：

> ```
> 星座,开始月日,结束月日,Unicode
> 水瓶座,120,218,9810
> 双鱼座,219,320,9811
> 白羊座,321,419,9800
> 金牛座,420,520,9801
> 双子座,521,621,9802
> 巨蟹座,622,722,9803
> 狮子座,723,822,9804
> 处女座,823,922,9805
> 天秤座,923,1023,9806
> 天蝎座,1024,1122,9807
> 射手座,1123,1221,9808
> 魔蝎座,1222,119,9809
> ```

#### 问题一：

请编写程序，读入CSV文件中数据，循环获得用户输入，直至用户输入 "exit" 退出。根据用户输入的星座名称，输出此星座的出生日期范围及对应字符形式。如果输入的星座名称有误，请输出“输入星座名称有误！”。

#### 参考代码

``` python
fo = open("SunSign.csv", "r", encoding="utf-8")
ls = []
for line in fo:
    line = line.replace("\n", "")
    ls.append(line.split(","))
fo.close()

while True:
    print("请输入星座名称，例如双子座,输入 exit 退出 \n")
    InputStr = input()    # 输入星座名称，例如双子座
    InputStr.strip()
    flag = False
    if InputStr == 'exit':
        break
    for line in ls:
        if InputStr == line[0]:
            print("{}座的生日位于{}-{}之间。".format(chr(eval(line[3])), line[1], line[2]))
            flag = True
    if not flag:
        print("输入的星座名称有误！")
```







