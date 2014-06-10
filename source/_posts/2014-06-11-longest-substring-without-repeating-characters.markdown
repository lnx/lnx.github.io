---
layout: post
title: "Longest Substring Without Repeating Characters"
date: 2014-06-11 01:35:08 +0800
comments: true
categories: ["en", "algorithm", "leetcode"]
---

**<a href="https://oj.leetcode.com/problems/longest-substring-without-repeating-characters/" class="external-link" target="_blank">Problem</a>**

_Given a string, find the length of the longest substring without repeating characters. For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3. For "bbbbb" the longest substring is "b", with the length of 1._

**Analysis**

Solve this problem with a queue like algorightm.

**Code**

{% codeblock Longest Substring Without Repeating Characters lang:java %}
public int lengthOfLongestSubstring(String s) {
	int ret = 0;
	int front = 0;
	int rear = -1;
	char[] chars = s.toCharArray();
	Set<Character> set = new HashSet<Character>();
	for (char c : chars) {
		if (set.contains(c)) {
			while (chars[front] != c) {
				set.remove(chars[front++]);
			}
			set.remove(chars[front++]);
		}
		rear++;
		set.add(c);
		int size = rear - front + 1;
		ret = ret > size ? ret : size;
	}
	return ret;
}
{% endcodeblock %}
