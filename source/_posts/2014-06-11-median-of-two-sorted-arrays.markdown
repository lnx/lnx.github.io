---
layout: post
title: "Median Of Two Sorted Arrays"
date: 2014-06-11 01:34:54 +0800
comments: true
categories: ["en", "algorithm", "leetcode"]
---

**<a href="https://oj.leetcode.com/problems/median-of-two-sorted-arrays/" class="external-link" target="_blank">Problem</a>**

_There are two sorted arrays A and B of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n))._

**Analysis**

This is a finding kth element problem. K is (A.length + B.length) / 2.

**Code**

{% codeblock Median Of Two Sorted Arrays lang:java %}
public double findMedianSortedArrays(int[] a, int[] b) {
	double ret = 0;
	int al = a.length;
	int bl = b.length;
	int len = al + bl;
	if ((len) % 2 == 0) {
		ret = (findKth(a, b, 0, al - 1, 0, bl - 1, len / 2 - 1) + findKth(a, b, 0, al - 1, 0, bl - 1, len / 2)) * 0.5;
	} else {
		ret = findKth(a, b, 0, al - 1, 0, bl - 1, len / 2);
	}
	return ret;
}

private int findKth(int[] a, int[] b, int as, int ae, int bs, int be, int k) {
	int ret = 0;
	int al = ae - as + 1;
	int bl = be - bs + 1;
	if (al == 0) {
		ret = b[bs + k];
	} else if (bl == 0) {
		ret = a[as + k];
	} else if (k == 0) {
		ret = a[as] < b[bs] ? a[as] : b[bs];
	} else {
		int am = k * al / (al + bl);
		int bm = k - am - 1;
		am += as;
		bm += bs;
		if (a[am] < b[bm]) {
			k -= am - as + 1;
			as = am + 1;
		} else {
			k -= bm - bs + 1;
			bs = bm + 1;
		}
		ret = findKth(a, b, as, ae, bs, be, k);
	}
	return ret;
}
{% endcodeblock %}
