## 算法相关

- 输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。
  思路：递归实现，如果当前节点不存在，返回0；否则返回较长子节点长度+1。
  ```js
  // 深度优先递归解法
  function TreeDepth(pRoot) {
  	return !pRoot ? 0 : Math.max(TreeDepth(pRoot.left), TreeDepth(pRoot.right)) + 1 
  }
  ```
  ```js
  // 广度优先非递归解法
	/* function TreeNode(x) {
	  this.val = x;
	  this.left = null;
	  this.right = null;
	} */
	function TreeDepth(pRoot)
	{
	    if (!pRoot) return 0
	    let result = 0
	    let tmp = []
	    tmp.push(pRoot)
	    while(tmp.length) {
		let tmp2 = []
		for (let i = 0; i < tmp.length; i++) {
		    if (tmp[i].left) tmp2.push(tmp[i].left)
		    if (tmp[i].right) tmp2.push(tmp[i].right)
		}
		tmp = tmp2
		result++
	    }
	    return result
	}
  ```
