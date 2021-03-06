SIGUSR1 SIGUSR2
================

## 问题：

* 怎么能快速实验下这两个信号？
* 为什么是`USR` 而不是 `USER` ？
* SIGUSR1、SIGUSR2 有什么惯例的使用方法吗？
* 做一个信号接收的最小DEMO演示

听说这两个信号是用来给“用户编程用的”，具体要做啥是用户来决定，从这个命名来看为了以后有`SIGUSER3` `SIGUSER4` ... 留了方案，虽然这一点认知没什么卵用。

听说linux有64个信号啊，NND太TMD多了，真的是好尴尬！


# 怎么能快速实验下这两个信号？



我觉得应该先找个一`python`的案例，直觉上觉得`python`从表面上更容易理解一点：

`receiveSIGUSR1.py`

```python 
import signal
import os
from time import sleep


# 处理信号的函数
def handlerSIGUSR1(signalNumber, stack):
    print("shutup!!!")


# 把信号`USR1` 交给 `handlerSIGUSR1` 来处理
signal.signal(signal.SIGUSR1, handlerSIGUSR1)
print(os.getpid())

# 为了让这个程序一直执行搞一个死循环
while (True):
    print("hi man")
    sleep(1)

```

代码出来内心还是安稳了很多....

奔跑起来吧 `python3 receiveSIGUSR1.py`

然后新开一个终端输入 `kill -USR1 {上边脚本打印出来的进程ID}`

还是很好玩哈哈。

从表象的认知来说，应该恐惧少了一点点...

执行的结果如下：

```
python3 receiveSIGUSR1.py
45988
hi man
hi man
hi man
hi man
hi man
hi man
hi man
hi man
hi man
shutup!!!
hi man
hi man
shutup!!!
hi man
shutup!!!
hi man
```

```
kill -s USR1 45988
kill -s USR1 45988
kill -s USR1 45988
```

# 为什么是`USR` 而不是 `USER` ？

为什么会有这个问题，因为我真的打错了好几次啊啊啊...

从看到`用户`两个字开始就从主观意识上认为这个就应该是`user` 而不是 `usr`

![why usr?](./images/SIGUSR1-why-not-user.jpg)

SIGUSR1 = Signal Unix Shared Resources 1?

SIGUSR1 = Signal user 1? 缩一个字母很难理解...

`User System Resources` ?

<http://www.gnu.org/software/libc/manual/html_node/Miscellaneous-Signals.html>



# 
