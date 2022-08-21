## 题意

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

---
## 解题

本题相较于上一题，改动的地方在于之字型打印，即，在逐行打印的时候，每一行的顺序需要有 从前到后 从后到前 的交替变化

```python
def levelOrder(self, root: TreeNode) -> List[List[int]]:
	if not root:
		return []
	direction=1
	stack=[root]
	res=[]
	while stack:
		curres=[]
		cur=[]
		for node in stack:
			curres.append(node.val)
			if node.left:
				cur.append(node.left)
			if node.right:
				cur.append(node.right)
		res.append(curres[::direction])
		direction=-direction
		stack=cur
	return res
```

```java
class Solution {
	public List<List<Integer>> levelOrder(TreeNode root) {
		Queue<TreeNode> queue = new LinkedList<>();
		List<List<Integer>> ans = new ArrayList<>();
		
		if (root != null) {
			queue.add(root);
		}
		
		while (!queue.isEmpty()) {
			List<Integer> curList = new ArrayList<>();
			for (int i = queue.size(); i > 0; i--) {
				TreeNode curNode = queue.poll();
				curList.add(curNode.val);
				if (curNode.left != null) {
					queue.add(curNode.left);
				}
				if (curNode.right != null) {
					queue.add(curNode.right);
				}
			}
			if (ans.size() % 2 == 1) {
				Collections.reverse(curList);
			}
			ans.add(curList);
		}
		return ans;
	}
}
```