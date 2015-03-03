---
layout: post
title: "正则表达式中的IP提取与质数判断"
date: 2015-03-03 13:05:59 +0800
comments: true
categories: ["regular-expression", "zh"]
---

本文介绍两个正则表达式的应用实例，希望能有所帮助。

## IP提取

判断IP是否合法的正则表达式网络上有很多，一般只需注意IP的数值边界就可以。以下就是Java中该类正则表达式的写法：

``` java
((2[0-4]\\d|25[0-5]|1\\d\\d|\\d\\d?)\\.){3}(2[0-4]\\d|25\\d|1\\d\\d|\\d\\d?)
```

但直接用上面的正则表达式抽取文本文件中的IP存在一个问题，虽然这里考虑了0-255边界的问题，但诸如“Current IP: 2255.0.0.1”中的“2255.0.0.1”还是会以“255.0.0.1”的方式被抽取出来。实际上这是一个不合法的IP，并不应该被匹配。

为了解决这个问题，考虑过用正则表达式中的零宽度匹配。不过这里给出的是另外一种解决方案，利用正则分组的方式。代码如下：

``` java IP Extract
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Test {

	List<String> extract(String line) {
		List<String> ret = new ArrayList<>();
		if (line != null) {
			String regex = "(^|[^\\d])(((2[0-4]\\d|25[0-5]|1\\d\\d|\\d\\d?)\\.){3}(2[0-4]\\d|25\\d|1\\d\\d|\\d\\d?))";
			Pattern p = Pattern.compile(regex);
			Matcher m = p.matcher(line);
			while (m.find()) {
				ret.add(m.group(2));
			}
		}
		return ret;
	}

	public static void main(String[] args) {
		String line = "Current IP: 192.168.1.1, 2255.0.0.1 and 1.0.0.0";
		System.out.println(new Test().extract(line));
	}

}
```

``` java Console Result
[192.168.1.1, 1.0.0.0]
```

## 质数判断

除了常规的字符串抽取以外，正则表达式还可以做一些以前我根本没有想到过的事情，比如这里的质数判断。

``` java Prime Valid
boolean valid(int num) {
	return num >= 0 && !String.format("%0" + num + "d", 0).matches("^0?$|^(00+?)\\1+$");
}
```

利用的是能否被其它数整除来做，思路挺巧妙的。具体解释这里就不作说明了，网络上有大量的介绍文章。每次看到这种近乎于炫技的代码，自己虽然不排斥，但过后总会思考，这些到底算不算茴香豆的四种写法。
