# 剑指Offer66题

## **28题： 数组中出现次数超过一半的数字**

### 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

解答如下：

```JAVA
//全排序实现，时间复杂度为快速排序复杂度O（nlogn）
import java.util.*;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        Arrays.sort(array);
        int arrLength=array.length;
        int p=arrLength/2;
        int k=array[p];
        for(int i=0;i<arrLength;i++){
            if(arrLength%2==0){
                if(i>=p){
                    break;
                }
            }else{
                    if(i>p){
                    break;
                }
            }
            if(array[i]==k&&array[i+p]==k)
            {
                return k;
            }
        }
        return 0;
    }
}
```

## 29题： 最小的K个数

## 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

解答如下：

```java
//全排序实现，时间复杂度为快速排序复杂度O（nlogn）
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        Arrays.sort(input);
        ArrayList<Integer> arraylist=new ArrayList<Integer>();
        if(k>input.length){
            return arraylist;
        }
        for(int i=0;i<k;i++){
        arraylist.add(input[i]);
        }
        return arraylist;
    }
}
//使用大根堆结构的优先队列实现，复杂度O（nlogk）,适合海量数据处理
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        Arrays.sort(input);
        ArrayList<Integer> arraylist=new ArrayList<Integer>();
        if(k>input.length||k==0){
            return arraylist;
        }
        PriorityQueue<Integer> maxheap=new PriorityQueue<Integer>(k,new Comparator<Integer>(){
            public int compare(Integer o1,Integer o2){
                return o2.compareTo(o1);
            }
        });
            for(int i=0;i<input.length;i++){
                if(maxheap.size()!=k){
                    maxheap.offer(input[i]);
                }else if(maxheap.peek()>input[i]){
                    Integer temp=maxheap.poll();
                    temp=null;
                    maxheap.offer(input[i]);
                }
            }
        for(Integer integer:maxheap){
            arraylist.add(integer);
        }
        return arraylist;
    }
}
```

## 3 0题： 连续子数组最大和

## 题目描述

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？(子向量的长度至少是1)

```java
//考察动态规划  f(i)=max(f(i-1)+array[i],array[i]);  maxint=max(maxint,f(i));
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        int maxint=array[0];
        int k=maxint;
        for(int i=1;i<array.length;i++){
            if(array[i]>k+array[i])
            {
                k=array[i];
            }else{
                k=k+array[i];
            }
            maxint=(k>maxint?k:maxint);
        }
        return maxint;
    }
}
```

## 3 1题： 整数中1出现的次数（从1到n整数中1出现的次数）

## 题目描述

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。

```java
//解题思路：（按位分析）举个例子345
// 10中个位出现1次数1次
// 100中十位出现1的次数10次
// 1000中百位出现1的次数100次
//那么10^n中出现1的次数为10^n-1
345中个位出现34*10^0+1=35次
345中十位出现(3+1)*10^1=40次
345中百位出现1*10^2=100次

public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) {
        int ones=0;
        for(int i=1;i<=n;i*=10){
            int a=n/i;
            int b=n%i;
            if(a%10==0){
                ones+=a/10*i;
            }else if(a%10==1){
                ones+=a/10*i+(b+1);
            }else{
                ones+=(a/10+1)*i;
            }
        }
        return ones;
    }
}
```

## 32题：把数组排成最小的数

## 题目描述

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

```java
import java.util.*;

public class Solution {
    public String PrintMinNumber(int [] numbers) {
        ArrayList<Integer> arraylist=new ArrayList<Integer>();
        for(int i=0;i<numbers.length;i++){
            arraylist.add(numbers[i]);
        }
        String s="";
        Collections.sort(arraylist,new Comparator<Integer>(){
            public int compare(Integer str1,Integer str2){
                String s1=str1+""+str2;
                String s2=str2+""+str1;
                return s1.compareTo(s2);
            }
        });
        for(int s1:arraylist){
            s+=s1;
        }
        return s;
    }
}
```

## 33题：丑数

## 题目描述

把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

```java
public class Solution {
    public int GetUglyNumber_Solution(int index) {
        if(index<7) return index;
        int res[]=new int[index];
        res[0]=1;
        int t1=0,t3=0,t5=0;
        for(int i=1;i<index;i++){
            res[i]=min(min(res[t1]*2,res[t3]*3),res[t5]*5);
            if(res[i]==res[t1]*2)t1++;
            if(res[i]==res[t3]*3)t3++;
            if(res[i]==res[t5]*5)t5++;
        }
        return res[(index-1)];
    }
    public int min(int a,int b){
        return a>b?b:a;
    }
}
```

## 34题：第一个只出现一次的字符

## 题目描述

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

```JAVA
public class Solution {

    public int FirstNotRepeatingChar(String str) {

        char[] chars = str.toCharArray();

        int[] map = new int[256];

        for (int i = 0; i < chars.length; i++) {

            map[chars[i]]++;

        }

        for (int i = 0; i < chars.length; i++) {

            if (map[chars[i]] == 1) return i;

        }

        return -1;

    }

}
```

## 35题：数组中的逆序对

## 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

## 输入描述:

```
题目保证输入的数组中没有的相同的数字数据范围：	对于%50的数据,size<=10^4	对于%75的数据,size<=10^5	对于%100的数据,size<=2*10^5
```

示例1

## 输入

```
1,2,3,4,5,6,7,0
```

## 输出

```
7
```

```java
//归并排序
public class Solution {

    public int InversePairs(int [] array) {
        if(array==null||array.length<=0){
            return 0;
        }
        int copy[]=new int[array.length];
        for(int i=0;i<array.length;i++){
            copy[i]=array[i];
        }
        int res=doinverse(array,copy,0,array.length-1);
        return res;
    }
    private int doinverse(int[] array,int[] copy,int start,int end){
        if(start==end){
            copy[start]=array[start];
            return 0;
        }
        int mid=(end+start)/2;
        int left=doinverse(copy,array,start,mid)%1000000007;
        int right=doinverse(copy,array,mid+1,end)%1000000007;
        int i=mid;
        int j=end;
        int copylength=end;
        int count=0;
        while(i>=start&&j>mid){
            if(array[i]>array[j]){
                count+=j-mid;
                count=count%1000000007;
                copy[copylength--]=array[i--];
            }else{
                copy[copylength--]=array[j--];
            }
        }
        for(;i>=start;i--){
            copy[copylength--]=array[i];
        }
        for(;j>mid;j--){
            copy[copylength--]=array[j];
        }
        for(int s=start;s<=end;s++){
            array[s]=copy[s];
        }
        return (left+count+right)%1000000007;
    }
}

```



## 36题：两个链表的第一个公共结点

## 题目描述

输入两个链表，找出它们的第一个公共结点。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
import java.util.*;
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode current1=pHead1;
        ListNode current2=pHead2;
        HashMap<ListNode,Integer> hashmap=new HashMap<ListNode,Integer>();
        while(current1!=null){
            hashmap.put(current1,null);
            current1=current1.next;
        }
        while(current2!=null){
            if(hashmap.containsKey(current2)){
                return current2;
            }else{
                current2=current2.next;
            }
        }
        return null;
        
    }
}
```

## 37题：数字在排序数组中出现的次数

## 题目描述

统计一个数字在排序数组中出现的次数。

```java
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
       int count=0;
        for(int i=0;i<array.length;i++){
            if(k==array[i]){
                count++;
            }
        }
        return count;
    }
}
```

## 38题：二叉树的深度

## 题目描述

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        int left=TreeDepth(root.left);
        int right=TreeDepth(root.right);
        return left>right?left+1:right+1;
    }
}
```

## 39题：平衡二叉树

## 题目描述

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

```java
public class Solution {
    boolean res=true;
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root==null){
            return true;
        }
        Isbalanced(root);
        return res;
    }
    private int Isbalanced(TreeNode root){
        if(!res){
            return 0;
        }
        if(root==null){
            return 0;
        }
        int left=Isbalanced(root.left)+1;
        int right=Isbalanced(root.right)+1;
        if(left-right>1||right-left>1){
            res=false;
        }
        return left>right?left:right;
    }
}
```





