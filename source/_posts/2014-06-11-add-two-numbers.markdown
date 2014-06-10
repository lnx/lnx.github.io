---
layout: post
title: "Add Two Numbers"
date: 2014-06-11 01:35:24 +0800
comments: true
categories: ["en", "algorithm", "leetcode"]
---

**<a href="https://oj.leetcode.com/problems/add-two-numbers/" class="external-link" target="_blank">Problem</a>**

_You are given two linked lists representing two non-negative numbers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list._

_Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)_

_Output: 7 -> 0 -> 8_

**Analysis**

This is a simple question. Take care of the carry digit.

**Code**

{% codeblock Add Two Numbers lang:java %}
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
	ListNode ret = new ListNode(0);
	ListNode cur = ret;
	int carry = 0;
	while (l1 != null) {
		if (l2 != null) {
			carry += l1.val + l2.val;
			l2 = l2.next;
		} else {
			carry += l1.val;
		}
		cur.next = new ListNode(carry % 10);
		cur = cur.next;
		carry /= 10;
		l1 = l1.next;
	}
	while (l2 != null) {
		carry += l2.val;
		cur.next = new ListNode(carry % 10);
		cur = cur.next;
		carry /= 10;
		l2 = l2.next;
	}
	while (carry > 0) {
		cur.next = new ListNode(carry % 10);
		cur = cur.next;
		carry /= 10;
	}
	return ret.next;
}
{% endcodeblock %}
