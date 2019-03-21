---
layout : post
title : 正则表达式---环视
category : 正则表达式
tags : 正则表达式
date: 2014-10-07
---
在正则表达式中，**断言用来声明一个应该为真的事实**，只有断言为真时才会继续进行匹配。不过要记得哦，断言只是匹配一个事实，而不是内容。这里介绍的断言，他们用于查找在某些内容之前或者之后，也就是一个位置应该满足的一定条件。

这里介绍四种断言：

*	顺序肯定环视 	`(?=exp)`
*	逆序肯定环视	`(?<=exp)`
*	顺序否定环视	`(?!exp)`
*	逆序否定环视	`(?<!exp)`

<!--more-->

##顺序肯定环视

断言自身出现的位置的后面只能匹配表达式exp.

比如，匹配"ing"结尾的单词前面部分（即除了ing以外的部分）

	\b\w+(?=ing\b)

上面表达式查找以下句子时，会匹配"sing"和"danc":
	
	I'm singsing while you're dancing.

##逆序否定环视

断言自身出现位置的前面智能匹配表达式exp。

比如，以re开头的单词的后半部分（即除了re以外的部分）

	(?<\bre)\w+\b

上面的表达式在查找以下句子时，会匹配"ading":

	I'm reading book.


下面这个例子同时使用以上两种环视操作，匹配以空格间隔的数字（**再次强调，不包括这些空格，断言是匹配的只是事实，而不是内容**）。

	(?<=\s)\d+(?=\s)

##顺序否定环视

断言此位置的后面不能匹配表达式exp。

1、匹配3位数字，而且这三位数字的后面不能是数字：

	\d{3}(?!\d)

2、匹配不包含连续字符串abc的单词：

	\b((?!abc)\w)+\b

#逆序否定环视

断言此位置的前面不能匹配表达式exp。

例如，前面不能是小写字母的7为数字：

	(?<![a-z])\d{7}

匹配不包括属性的简单HTML标签的内容：

	(?<=<(\w+)>).*(?=<\/\1>)

以上表达式最能表现断言的真正用途。`(?<=<(\w+)>)`指定前缀是被尖括号括起来的单词（比如`<b>`）,然后是`.*`实际内容（即任意的字符串），最后是一个后缀`(?=<\/\1>)`。注意后缀`\/`用到了字符转义，`\1`则是反向引用，引用的是捕获的第一组子表达式，即前面`(\w+)`的内容，如果前面匹配的是`<b>`，那么这里就是`</b>`。整个表达式匹配的就是`<b>`和`</b>`之间的内容（再次提醒，不包括前缀和后缀本身，他们只是一个事实）。

---

###总结

总体来说，环视相当于对所在位置附加了一个条件，**难点就在于找到这个位置**，这点解决了，那么环视就没什么神秘可言了！

（end）

