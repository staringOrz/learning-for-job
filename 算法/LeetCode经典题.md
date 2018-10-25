

# LeetCode经典题

### 859. 亲密字符串

给定两个由小写字母构成的字符串 `A` 和 `B` ，只要我们可以通过交换 `A` 中的两个字母得到与 `B` 相等的结果，就返回 `true` ；否则返回 `false` 。

 

**示例 1：**

```
输入： A = "ab", B = "ba"
输出： true
```

**示例 2：**

```
输入： A = "ab", B = "ab"
输出： false
```

**示例 3:**

```
输入： A = "aa", B = "aa"
输出： true
```

**示例 4：**

```
输入： A = "aaaaaaabc", B = "aaaaaaacb"
输出： true
```

**示例 5：**

```
输入： A = "", B = "aa"
输出： false
```

 

**提示：**

1. `0 <= A.length <= 20000`

2. `0 <= B.length <= 20000`

3. `A` 和 `B` 仅由小写字母构成。

   

   解答

```java
class Solution {
        public boolean buddyStrings(String A, String B) {
            int  a=A.length();
            int b=B.length();
            if(a!=b){
                return false;
            }
            int aa[]=new int[30];
            if(A.equals(B)){
                for(int i=0;i<A.length();i++){
                    aa[A.charAt(i)-'a']++;
                    if(aa[A.charAt(i)-'a']>1){
                        return true;
                    }
                }
                return false;
            }
            StringBuilder AA=new StringBuilder(A);
            int k[]=new int[3];
            int kk=0;
            for(int i=0;i<AA.length();i++){
                if(A.charAt(i)!=B.charAt(i)){
                    k[kk++]=i;
                }
                if(kk>2){
                    return false;
                }
            }
            if(kk!=2){
                return false;
            }
            char c=A.charAt(k[0]);
            AA.setCharAt(k[0],AA.charAt(k[1]));
            AA.setCharAt(k[1],c);
            if(AA.toString().equals(B)){
                return true;
            }
            return false;
        }
    }
```

### 856. 括号的分数

给定一个平衡括号字符串 `S`，按下述规则计算该字符串的分数：

- `()` 得 1 分。
- `AB` 得 `A + B` 分，其中 A 和 B 是平衡括号字符串。
- `(A)` 得 `2 * A` 分，其中 A 是平衡括号字符串。

 

**示例 1：**

```
输入： "()"
输出： 1
```

**示例 2：**

```
输入： "(())"
输出： 2
```

**示例 3：**

```
输入： "()()"
输出： 2
```

**示例 4：**

```
输入： "(()(()))"
输出： 6
```

 

**提示：**

1. `S` 是平衡括号字符串，且只含有 `(` 和 `)` 。
2. `2 <= S.length <= 50`

解答

```java
class Solution {
   public int scoreOfParentheses(String S) {
		if(S.length() == 0) return 0;
		if(S.charAt(0) == '(' && S.charAt(1) == ')') {
			return 1 + scoreOfParentheses(S.substring(2));
		}
		int d = 0;
		int end = -1;
		for(int i = 0; i < S.length(); i++) {
			if(S.charAt(i) == '(') d++;
			else d--;
			if(d == 0) {
				end = i+1;
				break;
			}
		}
		return 2 * scoreOfParentheses(S.substring(1, end-1)) + scoreOfParentheses(S.substring(end));
	}
}
```

### 858. 镜面反射

有一个特殊的正方形房间，每面墙上都有一面镜子。除西南角以外，每个角落都放有一个接受器，编号为 `0`， `1`，以及 `2`。

正方形房间的墙壁长度为 `p`，一束激光从西南角射出，首先会与东墙相遇，入射点到接收器 `0` 的距离为 `q` 。

返回光线最先遇到的接收器的编号（保证光线最终会遇到一个接收器）。

 

**示例：**

```
输入： p = 2, q = 1
输出： 2
解释： 这条光线在第一次被反射回左边的墙时就遇到了接收器 2 。
```

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/06/22/reflection.png) 

**提示：**

1. `1 <= p <= 1000`
2. `0 <= q <= p`

解答

```java
class Solution {
        public int mirrorReflection(int p, int q) {
            if(q==0)return 0;
            int cur=0,flag=0;
            while (true){
                flag++;
                cur+=q;
                cur%=(2*p);
                if(cur==p){
                    if(flag%2==1){
                        return 1;
                    }else {
                        return 2;
                    }
                }else if(cur==0)return 0;
            }
        }
    }
```

### 866. 具有所有最深结点的最小子树

[我的提交](https://leetcode-cn.com/contest/weekly-contest-92/problems/smallest-subtree-with-all-the-deepest-nodes/submissions/)[返回竞赛](https://leetcode-cn.com/contest/weekly-contest-92/)

- **用户通过次数**41
- **用户尝试次数**93
- **通过次数**42
- **提交次数**300
- **题目难度**Medium

给定一个根为 `root` 的二叉树，每个结点的*深度*是它到根的最短距离。

如果一个结点在**整个树**的任意结点之间具有最大的深度，则该结点是*最深的*。

一个结点的子树是该结点加上它的所有后代的集合。

返回能满足“以该结点为根的子树中包含所有最深的结点”这一条件的具有最大深度的结点。

 

**示例：**

```
输入：[3,5,1,6,2,0,8,null,null,7,4]
输出：[2,7,4]
解释：

我们返回值为 2 的结点，在图中用黄色标记。
在图中用蓝色标记的是树的最深的结点。
输入 "[3, 5, 1, 6, 2, 0, 8, null, null, 7, 4]" 是对给定的树的序列化表述。
输出 "[2, 7, 4]" 是对根结点的值为 2 的子树的序列化表述。
输入和输出都具有 TreeNode 类型。
```

 

**提示：**

- 树中结点的数量介于 1 和 500 之间。
- 每个结点的值都是独一无二的。

解答：

```java
int doit(TreeNode* root)
{
  int tmp1,tmp2;
  if (root==NULL) return 0;
  tmp1=doit(root->left);
  tmp2=doit(root->right);
  if (tmp1>tmp2) return tmp1+1;
  else return tmp2+1;
}

TreeNode* doit2(TreeNode* root,int dep)
{
  if (root==NULL) return NULL;
  if (dep==1) return root;
  TreeNode* tmp1=doit2(root->left,dep-1);
  TreeNode* tmp2=doit2(root->right,dep-1);
  if (tmp1==NULL) return tmp2;
  else if (tmp2==NULL) return tmp1;
  else return root;
}
 
class Solution {
public:
    TreeNode* subtreeWithAllDeepest(TreeNode* root) {
      int tmp=doit(root);
      return doit2(root,tmp);
    }
};


解答二
class Solution {
        int res=0;
        TreeNode resnode;
        public TreeNode subtreeWithAllDeepest(TreeNode root) {

            if(root==null)
                return root;
            if(root.left==null&&root.right==null){
                return root;
            }
            resnode=root;
            if(root.left!=null&&root.right!=null){
                if(maxDepth(root.left)==maxDepth(root.right)){
                    if(res<maxDepth(root.left)) {
                        return root;
                    }
                }
            }
            if(root.left==null&&root.right!=null){
                if(root.right.left==null&&root.right.right==null)
                {
                    if(res<1){
                        res=1;
                        resnode=root.right;
                    }
                }
            }
            if(root.right==null&&root.left!=null){
                if(root.left.left==null&&root.left.right==null)
                {
                    if(res<1){
                        res=1;
                        resnode=root.left;
                    }
                }
            }


            if(root.left!=null){
                subtreeWithDeepest(root.left, 1);
            }
            if(root.right!=null){
                subtreeWithDeepest(root.right, 1);
            }
            return resnode;
        }

        private void subtreeWithDeepest(TreeNode root, int i) {
             
            if(root.left!=null&&root.right!=null){
                if(res<i+1){
                    res=i+1;
                    resnode=root;
                }
                if(maxDepth(root.left)==maxDepth(root.right)){
                    if(res<maxDepth(root.left)+i) {
                        res=maxDepth(root.left)+i;
                        resnode=root;
                        return;
                    }
                }
            }
            if(root.left==null&&root.right!=null){
                if(maxDepth(root.right.left)==maxDepth(root.right.right))
                {
                    if(res<i+maxDepth(root.right)){
                        res=i+maxDepth(root.right);
                        resnode=root.right;
                        
                        return;
                    }
                }
            }
            if(root.right==null&&root.left!=null){
                if(maxDepth(root.left.left)==maxDepth(root.left.right))
                {
                    if(res<i+maxDepth(root.left)){
                        res=i+maxDepth(root.left);
                        resnode=root.left;
                        
                        return;
                    }
                }
            }
            if(root.left!=null){
                subtreeWithDeepest(root.left, i+1);
            }
            if(root.right!=null){
                subtreeWithDeepest(root.right, i+1);
            }
        }
        public int maxDepth(TreeNode root) {
            int ress=0;
            if(root==null){
                return ress;
            }
            int left=1;
            if(root.left!=null){
                left=maxDepth(root.left)+1;
            }
            int right=1;
            if(root.right!=null){
                right=maxDepth(root.right)+1;
            }

            ress=left<right?right:left;
            return ress;
        }
    }
```

##  旋转数组

给定一个数组，将数组中的元素向右移动 *k* 个位置，其中 *k* 是非负数。

**示例 1:**

```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

**示例 2:**

```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

**说明:**

- 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
- 要求使用空间复杂度为 O(1) 的原地算法。

```java 

class Solution {
    public void rotate(int[] nums, int k) {
        // Three solutions
        // solution 1
        if(nums == null || nums.length == 0 || k == 0) return;
        int len = nums.length;
        k = k % len;
        reverse(nums, 0, len - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, len - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        if(start >= end) return;
        int mid = start + (end - start) / 2;
        for(int i = start; i <= mid; i++) {
            swap(nums, i, start + end - i);
        }
    }
    public void swap(int[] nums, int first, int second) {
        int temp = nums[first];
        nums[first] = nums[second];
        nums[second] = temp;
    }
}
```



