---
layout: post
title: "Morris二叉树遍历算法"
date: 2014-11-26 18:08:46 +0800
comments: true
categories: ["algorithm", "zh"]
---

## 背景

二叉树的前中后序遍历算法是计算机领域的基础算法，一般采用递归或者栈来实现。时间复杂度为O(n)，空间复杂度为O(logn)。1968年，Knuth提出说能否将该问题的空间复杂度压缩到O(1)，同时原树的结构不能改变。大约十年后，1979年，Morris在《Traversing Binary Trees Simply and Cheaply》这篇论文中用一种Threaded Binary Tree的方法解决了该问题。

## Threaded Binary Tree

为了实现O(1)空间复杂度的遍历，Threaded Binary Tree对普通二叉树进行了一些改造，将每一个节点在中序遍历时的前驱节点的右子树指向自己。说起来比较绕口，不过参考下面的示意图就会马上明白是怎么回事。

{% img /images/2014/threaded_binary_tree.jpg %}

Morris算法在遍历过程中动态的构建Threaded Binary Tree，同时在结束时又将树恢复原样，在满足O(1)空间复杂度的同时也恰好满足Knuth对树结构不能改变的要求。

## 前序与中序遍历

下面给出前序遍历的Morris实现，程序最核心的部分是寻找每个节点的前驱节点，并根据前驱节点右子树是否为空来决定当前节点是否被访问过。



``` java Preorder Traversal
List<Integer> preOrder(TreeNode root) {
	List<Integer> ret = new ArrayList<>();
	TreeNode cur = root;
	while (cur != null) {
		if (cur.left == null) {
			ret.add(cur.val);
			cur = cur.right;
		} else {
			TreeNode pre = cur.left;
			while (pre.right != null && pre.right != cur) {
				pre = pre.right;
			}
			if (pre.right == null) {
				pre.right = cur;
				ret.add(cur.val);// 前序遍历
				cur = cur.left;
			} else {
				pre.right = null;
				cur = cur.right;
			}
		}
	}
	return ret;
}
```

中序遍历与前序遍历相似，只需要将第15行的 ret.add(cur.val) 添加到第19行 cur=cur.right 的前面就可以了。掌握了前序和后序遍历的O(1)空间复杂度实现，大家可以暂时停下来想一想，我们该如何实现后序遍历？

## 后序遍历

算法思想与前序和中序遍历一致，只不过我们需要添加一个新的根节点，这个新的根节点的左子树为原树的根节点，右子树为空。假设当前节点为cur，在遍历完了cur.left的左子树以后，我们逆向遍历从cur.left到cur的中序遍历前驱节点间的所有节点，这样就可以实现cur的左子树的后序遍历。因为最开始我们添加了一个新的根节点，它的左子树是原树，所以可以保证最终我们能够得到整个树的后序遍历。

``` java Postorder Traversal
List<Integer> postOrder(TreeNode root) {
	List<Integer> ret = new ArrayList<>();
	TreeNode cur = new TreeNode(-1);
	cur.left = root;
	while (cur != null) {
		if (cur.left == null) {
			cur = cur.right;
		} else {
			TreeNode pre = cur.left;
			while (pre.right != null && pre.right != cur) {
				pre = pre.right;
			}
			if (pre.right == null) {
				pre.right = cur;
				cur = cur.left;
			} else {
				pre.right = null;
				reverse(cur.left);
				TreeNode node = pre;
				while (node != null) {
					ret.add(node.val);
					node = node.right;
				}
				reverse(pre);
				cur = cur.right;
			}
		}
	}
	return ret;
}
```

``` java Reverse
void reverse(TreeNode node) {
	TreeNode cur = null;
	while (node != null) {
		TreeNode next = node.right;
		node.right = cur;
		cur = node;
		node = next;
	}
}
```

## 时间复杂度

表面上看我们的程序中包含有两层的while循环，但实际上Morris算法的时间复杂度仍然是O(n)。对于前序和中序遍历，假设有n个节点，二叉树中的n-1条边每条边最多被访问2次。第一次是确定当前节点的前驱节点，第二次是从前驱节点返回到当前节点以后的再次访问。所以总体上来看，算法复杂度是O(2n)=O(n)。

对于后序遍历，因为比前序和中序遍历多了两次反转操作(reverse)，这就导致每条边最多被访问4次，最终算法复杂度是O(4n)=O(n)。

## 总结

Morris算法虽然在时间复杂度上有着系数级别的差异，但却带来了空间复杂度量级上的降低。总体看来，在某些空间苛刻的场景中，该算法非常实用。
