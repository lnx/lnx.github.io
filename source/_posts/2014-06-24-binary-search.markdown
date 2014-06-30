---
layout: post
title: "二分查找"
date: 2014-06-24 21:39:09 +0800
comments: true
categories: ["algorithm", "zh"]
---

{% img /images/2014/knuth.jpg %}

**Donald Knuth** -- _a computer scientist_

## 逸闻趣事

计算机领域内人才济济，各路神仙竞相释放大招。创世神们的事迹就不一一列举了，单是那一个个优美的小算法，在我眼中也是一件件上古奇兵。

对没错，前面这段是欲扬先抑。虽然领域内的能人堪比北京的处级干部般多如牛毛，但依旧阻挡不住下面这件令我震惊的事的发生：

> _Donald Knuth在叙述查找算法的历史时指出，尽管第一篇有关二分查找（折半查找）算法的文章在1946年就发布了，可16年后才有人发表了（这是一个很好的限定）能正确查找各种规模的列表算法。_

诸位感受下，这16年来的计算机界（至少是学术界），竟然连个简单的折半查找都没正确的实现出来。这至少说明有相当一部分的从业人员算法基本功是不过关的。当然，你可以说这不重要。但我觉得算法便如武侠世界中的内功，练得好绝对对以后修炼各派神功大有裨益。

说起老爷子，还有一段逸闻趣事值分享。当年老爷子觉得现有的排版系统不好开始制造Tex时，第一版的源码老爷子是写在一个很厚的笔记本上的！请注意，这里是一个真正意义上的纸质笔记本。

> _When I wrote TeX originally in 1977 and ’78, of course I didn’t have literate programming but I did have structured programming. I wrote it in a big notebook in longhand, in pencil._

> _Six months later, after I had gone through the whole project, I started typing into the computer. And did the debugging in March of ’78 while I had started writing the program in October of ’77. The code for that is in the Stanford archives—it’s all in pencil—and of course I would come back and change a subroutine as I learned what it should be._

看到这里时我已热泪盈眶，这六个月高老爷子到底是如何组织自己的代码的，这才应该是真正意义上的最强大脑。当然也可以理解这是一种偏执，见仁见智罢了。有时候不去过分的迷信权威其实对于破立是非常有好处的（推荐阅读王垠的<a href="http://www.yinwang.org/blog-cn/2014/01/04/authority/" target="_blank">《我和权威的故事》</a>）。

## 那些坑

回到正题，接着说二分查找。究竟这里的二分查找难在什么地方，竟引得无数英雄折腰于此！我觉得这个问题至少存在以下3个坑（对那些基本功扎实的程序员来说：空中飘来五个字，这都不是事儿！）：

**1.中值的计算方式**

这里最容易忽略的就是溢出问题。如果我们理所应当的认为，中值应该这样计算：

{% codeblock lang:c %}
int mid = (high + low) / 2;
{% endcodeblock %}

那么就肯定会被这种方式甩出一条街：

{% codeblock lang:c %}
int mid = low + (high - low) / 2;
{% endcodeblock %}

当然，如果你用这种方式，那明显会更好一些：

{% codeblock lang:c %}
int mid = low + (high - low) >> 1;
{% endcodeblock %}

这里核心的问题就是在避免两数相加以后 int 溢出的问题，后面的两种方式就很好的避免了这种情况的发生。第三种方式因为采用了位运算，在速度上更优。当然，别习惯性的在这里 >>2 就可以了。

**2.选择区间是否对称**

这又是一个让很多人栽倒的地方，边界问题考虑稍微有点疏忽，很容易就会掉进坑里。假如我们这里选择定义一个闭区间的话，那应该这样写：

{% codeblock lang:c %}
int low = 0, high = n - 1;
{% endcodeblock %}

相应的循环体应该是这样的：

{% codeblock lang:c %}
while (low <= high) {
	// TODO
}
{% endcodeblock %}

当然，这里你也可以写成一个半开半闭的区间：

{% codeblock lang:c %}
int low = 0, high = n;
{% endcodeblock %}

但对应的循环体就应该是这样的：

{% codeblock lang:c %}
while (low < high) {
	// TODO
}
{% endcodeblock %}

好多人在这里定义的时候想当然，没有仔细考虑边界，在循环条件是 <= 还是 < 的时候掉进了坑里。另外，如果这里采用指针的话，还存在另外一个问题，第一种闭区间的写法在计算 n-1 的时候容易指向一个无效地址：

{% codeblock lang:c %}
int *low, *high;
low = a;
high = a + n - 1;
{% endcodeblock %}

如果这个时候 n 是 0 的话，那又会妥妥的掉坑里。不过，这种情况对于半开半闭区间不存在问题。

**3.重复元素的处理**

大部分人在这里理所当然的认为找到就应该直接跳出循环体，根本没有考虑到重复元素这种情况。如果要返回的是第一个元素的话，那么简单的跳出肯定有问题。

最终的实现如下：

{% codeblock Binary Search lang:java %}
public int binarySearch(int[] a, int v) {
	int low = -1, high = a.length;
	while (low + 1 < high) {
		int mid = low + ((high - low) >> 1);
		if (v > a[mid]) {
			low = mid;
		} else {
			high = mid;
		}
	}
	return high < a.length && a[high] == v ? high : -1;
}
{% endcodeblock %}

## 小结

学习各种新的技术无疑对开阔眼界大有帮助，也可以让自己的工具盒里有着各式各样顺手的工具。但基础依旧是基础，经典依旧是经典。数据结构、算法、编译器、程序语言设计、操作系统等，对这些内容掌握的熟练程度，某种意义上决定了你在整个梯队中的位置。
