---
layout: post
title: "ZigZag Conversion"
date: 2014-06-11 01:35:46 +0800
comments: true
categories: ["en", "algorithm", "leetcode"]
---

**<a href="https://oj.leetcode.com/problems/zigzag-conversion/" class="external-link" target="_blank">Problem</a>**

_The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)_

	P   A   H   N
	A P L S I I G
	Y   I   R

_And then read line by line: "PAHNAPLSIIGYIR"_

_Write the code that will take a string and make this conversion given a number of rows:_

	string convert(string text, int nRows);

_convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR"._

**Analysis**

This is an easy one. Just do your best to keep code clean.

**Code**

{% codeblock ZigZag Conversion lang:java %}
public String convert(String str, int rows) {
	StringBuilder ret = new StringBuilder();
	char[] chars = str.toCharArray();
	int size = rows == 1 ? 1 : (rows - 1) * 2;
	for (int i = 0, len = chars.length; i < rows; i++) {
		for (int j = i; j < len; j += size) {
			ret.append(chars[j]);
			if (i != 0 && i != rows - 1) {
				int next = j + size - 2 * i;
				if (next < len) {
					ret.append(chars[next]);
				}
			}
		}
	}
	return ret.toString();
}
{% endcodeblock %}
