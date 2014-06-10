---
layout: post
title: "Longest Palindromic Substring"
date: 2014-06-11 01:35:35 +0800
comments: true
categories: ["en", "algorithm", "leetcode"]
---

**<a href="https://oj.leetcode.com/problems/longest-palindromic-substring/" class="external-link" target="_blank">Problem</a>**

_Given a string S, find the longest palindromic substring in S. You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring._

**Analysis**

Don't forget this: the final length of palindromic substring could be odd or even.

**Code**

{% codeblock Longest Palindromic Substring lang:java %}
public String longestPalindrome(String str) {
	String ret = "";
	if (str != null) {
		char[] chars = str.toCharArray();
		for (int i = 0, len = chars.length; i < len; i++) {
			int left = len - (i + 1);
			int step = i < left ? i : left;
			for (int j = 0; j < step; j++) {
				if (chars[i - j - 1] != chars[i + j + 1]) {
					step = j;
					break;
				}
			}
			if (step * 2 + 1 > ret.length()) {
				ret = str.substring(i - step, i + step + 1);
			}
			step = (i + 1) < left ? (i + 1) : left;
			for (int j = 0; j < step; j++) {
				if (chars[i - j] != chars[i + j + 1]) {
					step = j;
					break;
				}
			}
			if (step * 2 > ret.length()) {
				ret = str.substring(i - step + 1, i + step + 1);
			}
		}
	}
	return ret;
}
{% endcodeblock %}
