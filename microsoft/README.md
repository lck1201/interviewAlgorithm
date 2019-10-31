# 微软面试

<details>

<summary>目录</summary>

- [微软面试](#%e5%be%ae%e8%bd%af%e9%9d%a2%e8%af%95)
  - [一般流程](#%e4%b8%80%e8%88%ac%e6%b5%81%e7%a8%8b)
  - [算法题](#%e7%ae%97%e6%b3%95%e9%a2%98)
    - [数学](#%e6%95%b0%e5%ad%a6)
    - [字符串处理](#%e5%ad%97%e7%ac%a6%e4%b8%b2%e5%a4%84%e7%90%86)
    - [二分查找](#%e4%ba%8c%e5%88%86%e6%9f%a5%e6%89%be)
    - [并查集](#%e5%b9%b6%e6%9f%a5%e9%9b%86)
    - [maxStack](#maxstack)
    - [树](#%e6%a0%91)
    - [动态规划](#%e5%8a%a8%e6%80%81%e8%a7%84%e5%88%92)
    - [贪心](#%e8%b4%aa%e5%bf%83)
    - [√two pointers](#%e2%88%9atwo-pointers)
    - [大数据](#大数据)
  - [逻辑题](#%e9%80%bb%e8%be%91%e9%a2%98)
    - [√ 概率题（用少表示多并均匀）](#%e2%88%9a-%e6%a6%82%e7%8e%87%e9%a2%98%e7%94%a8%e5%b0%91%e8%a1%a8%e7%a4%ba%e5%a4%9a%e5%b9%b6%e5%9d%87%e5%8c%80)
    - [√ 凸包问题](#%e2%88%9a-%e5%87%b8%e5%8c%85%e9%97%ae%e9%a2%98)
      - [求包裹所有点的凸多边形](#%e6%b1%82%e5%8c%85%e8%a3%b9%e6%89%80%e6%9c%89%e7%82%b9%e7%9a%84%e5%87%b8%e5%a4%9a%e8%be%b9%e5%bd%a2)
  - [软技能(行为面试)](#%e8%bd%af%e6%8a%80%e8%83%bd%e8%a1%8c%e4%b8%ba%e9%9d%a2%e8%af%95)
</details>

## 算法题

不难，**交流很重要，要互动inplace or not，树里面有没有负数！！！**注重**代码质量（编程风格、命名规范、异常值处理）**。各种**follow up**以及要你设计尽可能多的**测试用例**。

补充，外企高频算法题[https://zhuanlan.zhihu.com/p/21474308](https://zhuanlan.zhihu.com/p/21474308)

### 数学
* **高频题**
  * ✔️[旋转图像](https://leetcode.com/problems/rotate-image)++++
    * Q: inplace?方阵？
```c++
 //* clockwise rotate
  * first reverse up to down, then swap the symmetry 
  * 1 2 3     7 8 9     7 4 1
  * 4 5 6  => 4 5 6  => 8 5 2
  * 7 8 9     1 2 3     9 6 3 *//
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

  * ✔️[涂色问题](https://blog.csdn.net/BBHHTT/article/details/79759509)+++
    * 给定无向连通图G和m种不同的颜色，用这些颜色给图的各个顶点着一种颜色，若某种方案使得图中每条边的2个顶点的颜色都不相同，则是一个满足的方案，找出所有的方案。
    * 思路：
      * DFS， 复杂度o\(n^3\)
      * 写一个函数判断某个点和连接的点是否颜色相同，即判断matrix\[x\]\[y\] == 1 and color\[x\] == color\[y\]，遍历y，找到不同的就可以返回1.

```c++
int sameColorWithNeighbour(int x) {  //判断x顶点的颜色是否与邻接点相同
 for(int i=1;i<=n;i++)
  if(mat[i][x]==1&&cloor[i]==cloor[x])
   return 0;
 return 1;  //不相同，可以选择
}
void dfs(int pIndex){
 if(pIndex==n+1){//共n个顶点
  sum++;
  return;
 }
 for(int i=1;i<=m;i++){
  cloor[pIndex]=i;  //先确定颜色，再看是否重复
  if(sameColorWithNeighbour(pIndex)) dfs(pIndex+1); //下个顶点进行选择
  cloor[pIndex]=0; //回溯
 }
}
//entry, must init graph, color vector
dfs(1)
```

* ✔️找数组中和为target+++
    * [找出一个数组中和为target两个数的索引](https://leetcode.com/problems/two-sum/)++
    * 把target-当前数的值存进字典，value是当前的索引。
    * 当后面的数值在字典中，说明这个数和之前某个数之和为target，取出之前的索引
    * follow up：找出一个数组里和为target的三元组, **reduce成 2Sum 问题**
    * follow up: 找出一个数组中和为target的组合, **reduce成 2Sum 问题**

* 在一个数组里找任意两个数之和的绝对值最小值
    * Q: 可以排序？可以
    * 特例判断：排序后min是正数、max是负数；用双指针，注意指针移动条件（Must：左指针指向负数、右指向正数

* **✔️最长连续子序列**
  * 给一个只包含0和1的序列，找出保证0和1的数目相同的最长连续子列（从头开始）。[http://www.voidcn.com/article/p-pshuooqw-mn.html](http://www.voidcn.com/article/p-pshuooqw-mn.html)
  * 把0变成-1，题目等同于求和为0的最长连续子序列，动态规划，注意在DP表内要找最后一个**偶数位**的0
  * 如果子串不是从头开始怎么办？比如1101100，此时动态规划方法要调整。
    * 对每个子列和进行map映射，找相同的值的最大index之差。
    * 比如1101100
    * ![](index_files/bd1b2d39-0fa3-45d9-b968-96bdd7675372.png)
* **相亲数**
  * amicable number, 两个正整数中，彼此的全部正约数之和与另一方相等。 遍历，难在概念理解。[https://blog.csdn.net/weijifen000/article/details/79598216](https://blog.csdn.net/weijifen000/article/details/79598216)
    * 约数又叫因数，b是a的约数，则a%b==0，整除没有余数
    * 220的所有因子\(除了自己以外的因子\)：1+2+4+5+10+11+20+22+44+55+110 = 284， 284的所有因子：1+2+4+71+142 = 220
    * 注意 1）6的因数和是6，但不能算 2）注意平方数，其平方根算一次
* **全排列**
  * 找到这个permutation在全排列中的index。[https://leetcode.com/problems/permutations/discuss/18296/Simple-Python-solution-\(DFS\).](https://leetcode.com/problems/permutations/discuss/18296/Simple-Python-solution-%28DFS%29.)

<del>* 一道是长度为N的数组，每个元素为0或1或2，找到满足a[i]< a[j]<[k]的(i, j, k)的数量</del>
* 一个数组中只有1，2，3三种元素，要求对这样的数组进行排序：三个指针
    * p1从左侧开始，指向第一个非1的数字；p3从右侧开始，指向第一个非3的数字。
        1.  p2从p1开始遍历，如果是2，p2继续遍历，直到p2遇到1或者3
        2.  如果遇到1，则和p1进行交换，然后p1向右，指向第一个非1的数字。
        3.  如果遇到3，则和p3进行交换，然后p3向左，指向第一个非3的数字。
* **简单题：**
  * 求1到1000000的质数。筛法是指假设所有数都为素数（dp table，init as true），然后遍历，如果其为素数，则其倍数皆为和数（dp table set as false），遍历所有数即可。 [解法](https://blog.csdn.net/Quincuntial/article/details/79016354)
  * 两个大数相加，思路：
       1. 反转两个字符串，便于从低位到高位相加和最高位的进位导致和的位数增加；
       2. 对齐两个字符串，即短字符串的高位用‘0’补齐，便于后面的相加；(或者新开一个ans数组)
       3. 把两个正整数相加，一位一位的加并加上进位。arr3[i] = (carry + arr1[i] + arr2[i])%10, carry = (carry + arr1[i] + arr2[i]) / 10
  * 大整数相乘。[【算法】大数乘法问题及其高效算法](https://itimetraveler.github.io/2017/08/22/%E3%80%90%E7%AE%97%E6%B3%95%E3%80%91%E5%A4%A7%E6%95%B0%E7%9B%B8%E4%B9%98%E9%97%AE%E9%A2%98%E5%8F%8A%E5%85%B6%E9%AB%98%E6%95%88%E7%AE%97%E6%B3%95/) - 见模拟乘法累加 - 改进
  * 求数组中所有数拼成的最大数。Leetcode 179, 思路：定义数组排序规则即可。python可重载比较函数，注意python3: `_nums.sort(key=functools.cmp_to_key(compare), reverse=True)`
  * 判断三个数能否形成三角形，要求实现`isTriangle(int a, int b, int c)` 思路：只能判断相减是否小于，不能用两边之和大于第三边，会溢出。
  * 给定一个函数rand100，它可以随机返回1 - 100的一个整数，要求用这个函数实现rand10000。`A: 100 * rand100() - rand100() + 1`
      * follow up: 用rand100\(\)实现rand500\(\)  `A: 5 * rand100() - ((rand100() % 5)`
      * follow up2: 用rand100\(\)实现rand150\(\) `A: 3 * (rand100() % 50 + 1) - (rand99() % 3)`
      * 结论：如果x和y可以用rand100在确定时间内生成，那么给定一个数，问能不能用rand100在确定时间内生成。动态规划`canForm[i] && canForm[j]，那么canForm[i * j] = true`
      * 通解---步骤1： 如果a > b，进入步骤2；否则构造Randa2 = a * (Randa – 1) + Randa，表示生成1到a2 随机数的函数。如果a2 仍小于b，继教构造 Randa3 = a * (Randa2 – 1) + Randa…直到ak > b, 这时我们得到Randak , 我们记为RandA。
     步骤2：步骤1中我们得到了RandA(可能是Randa或Randak)，其中A > b
     另外：从上面一系列的分析可以发现，如果给你两个生成随机数的函数Randa和Randb， 你可以通过以下方式轻松构造Randab，生成1到a*b的随机数。1. Randab = b * (Randa - 1) + Randb 2. Randab = a * (Randb - 1) + Randa
      * [讲解1](https://www.jianshu.com/p/f540a428d190)，[讲解二](https://blog.csdn.net/wangruitao1991/article/details/51678815)
* 数组中的第k大的数。思路：用最小堆。时间复杂度：O(K) + O(log(N-K))

### 字符串处理

* 字母和数字的映射问题：给定一个字母和数字的映射关系，如 1-26 对应a-z，然后输入一个数字型的字符串如“123”，请输出所有可能的字母型字符串，“abc”和“aw”和”lc”。Leetcode91，剑指offer也有，但leetcode这个解法更好。
    ```python
    dp = [0] * (len(s) + 1)
    dp[0] = 1
    dp[1] = 1 if s[0] != '0' else 0

    for i in range(1, len(s)):
      onePhase = int(s[i:i + 1])  #start loop from s[1], correspond to dp[2]
      twoPhase = int(s[i - 1:i + 1])
        if onePhase >= 1 and onePhase <= 9:
      dp[i + 1] += dp[i]
        if twoPhase >= 10 and twoPhase <= 26:
      dp[i + 1] += dp[i - 1]</pre>
    ```
* [atoi函数](https://leetcode.com/problems/string-to-integer-atoi/)，字符串转整数
  * `问清楚规则，包括 1E10，负号，数组部分还是全部，非法字符等等`
  * 去空格，strip\(\)
  * 字符串是否为空，空的话回0
  * 超出2^31-1或者-2^31，全都截断成2^31-1或者-2^31
  * 第一个字符正负，获取并保存符号
  * 获取数字子串，直到发现非数字字符，退出循环
* [https://leetcode.com/problems/word-break/description/](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#) Q.139。问"leetcode"能不能break成['leet','code'](单词可以用 >=0 次）
    ```python
    # dp表含义：以i结尾的subString是否在dict里
    for i in range(len(s)):
        for w in words:
            if w == s[i - len(w) + 1:i + 1] and (d[i - len(w)] or i - len(w) == -1):
                d[i] = True
    ```
    动态规划，DP表：以i为结尾的subString是否可以由wordDict中的word组成。d[i] is True if there is a word in the dictionary that ends at ith index of s AND d is also True at the beginning of the word
* [https://leetcode.com/problems/integer-to-english-words/](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#) Q. 273 [Brilliant Java Solution](https://leetcode.com/problems/integer-to-english-words/discuss/70625/My-clean-Java-solution-very-easy-to-understand)
* 有一个只包含字母和空格的文本，输出其中所有的单词，不能用STL。A: 指针移动，用一个buffer记录当前字母，遇空格保存buffer 并清空。
* 给一个字符串，是一个普通的英语句子，要求把里面所有单词的出现顺序反序，但是单词本身不反序，例如"This is a test" -&gt; "test a is This" **A:先句子反转，再单词反转。实现函数reverseString（ptr1,ptt2），利用双指针**
  * 注意开头结尾和中间有连续空格
  * 注意标点符号
* 给一个字符串，有大写和小写字母，要把所有大写字母移动到小写字母后面并保持顺序不变。 note 问清楚题目要求，时间，空间。A: 双指针，从尾巴开始往前移动，ptr1保存大写字母，ptr2往前移动找大写字母。ptr1从右往左找第一个小写字母，ptr2初始化为ptr1。插入，不是交换。另一方法：正则表达式匹配。
* 给一个字符串求最少删掉几个字符可以变成回文串。A:先求字符串s的反转串rs，然后求s和rs的最大公共子序列（子序列和子串的区别是子序列不要求连续，子串要求连续），则最大公共子序列的长度即为最大回文串的长度。[Leetcode解析](https://leetcode.com/problems/longest-common-subsequence/discuss/348884/C%2B%2B-with-picture-O(nm))
```
# LCS算法，dp表意思：分别，前i个和前j个的LCS。
if text1[i - 1] == text2[j - 1]: dpTable[i][j] = dpTable[i - 1][j - 1] + 1
else: dpTable[i][j] = max(dpTable[i - 1][j], dpTable[i][j - 1])
```
<del>* 给 定两个字符串S1 S2，如果f\(S1.substring\)=S2.substring，且 f: 改变字符串中的一个字符。请找出所有的S1.substring edit distance?</del>
* [最长回文子串](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#)
* 输入一个char 与 str，要求把str中所有的"ab"都替换成"c"，把所有单个的"b"都替换成"ef"。要求in-place，并且保证str中"ab"的个数 &gt;"b"的个数（也就是str的长度足够放下替换后的结果）A：遍历多次可解，但问清楚替换后出现的空格怎么处理。需要问清时间复杂度。

### 二分查找

* +++剑指offer原题n\*m的有序数组（每行/每列）都是递增数列中如何最快的查找一个数字。 `A:剑指offer，二维数组的查找。`
* 给定一个数组，其中的元素先递减后递增，要求找出最小的那个元素的下标。 `A: 1）线性方法，ans初始化为nums[0], if nums[i+1] < nums[i]: ans = nums[i+1]。 2)二分方法：中点位于递增的arr，还是递减的arr。`
* ++Seach in rotated array[https://leetcode.com/problems/search-in-rotated-sorted-array/discuss/14425/Concise-O(log-N)-Binary-search-solution](https://leetcode.com/problems/search-in-rotated-sorted-array/discuss/14425/Concise-O(log-N)-Binary-search-solution) `A:**先找到旋转点**，然后看是在左arr还是右arr，再用二分法`
* partition算法！！**重要！！二分方法的core，需要背出来**
  * 给定一个乱序的链表，给定一个数字，把比这个数字大的结点放在后面，比这个数字小的结点放在前面，要求稳定性，即保持原数字之间的相对位置。
  * 找出数组中的中位数 `思路：partitionIdx和middle比较，然后继续二分`
* +++[top k](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#) `A:最小堆，堆可以理解为二叉树，但实际用数组实现，序号为确定的值(2i+1, 2i+2为两个子节点的index)。建堆O(N)，操作log(N)`
* 在时间复杂度O(log(m+n))的要求下寻找两个有序数组合并后的中位数；A: [【分步详解】两个有序数组中的中位数和Top K问题](https://blog.csdn.net/hk2291976/article/details/51107778) 略微有点复杂。
* [merge sorted array](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#)\(easy\) `(剑指offer有)从较长的数组的末尾开始填充，把两个数组末尾中较大的填充到最后，避免移动数组元素。`

### 并查集
[https://blog.csdn.net/liujian20150808/article/details/50848646](https://blog.csdn.net/liujian20150808/article/details/50848646)
掌握三件事：1.find函数：找到x的root，2.路径压缩：可以在find函数里写，就是直接将x的pre设置为root 3.join函数，将给定的x与y的两个root连在一起

* +++[https://leetcode.com/problems/friend-circles/](https://leetcode.com/problems/friend-circles/) `A:可以用并查集，也可以DFS + visited记录`
* [https://leetcode.com/problems/accounts-merge/](https://leetcode.com/problems/accounts-merge/) A: 解1 [DFS+HashMap Solution](https://leetcode.com/problems/accounts-merge/discuss/109161/Python-Simple-DFS-with-explanation!!!) 建立email->owner(用id表示) dict;start from one account/email; dfs this email; use visited控制当前dfs时访问过的一个account； 解2[Union Find Solution](https://leetcode.com/problems/accounts-merge/discuss/109157/JavaC%2B%2B-Union-Find)1）parent(email) = email自己，owner(email) = name； 2）parent（email) = list中第一个email； 3）以第一个email建String->Set()的表，将所有以它为root的email归到Set中（用find找root）；4）遍历表中的每一个key，即email name，用Owner.get(email)得到名字，Set组成email，放到结果中
* ++ [https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/) A: 解1 最外层遍历所有点，ans += 1 if 当前点为1，用DFS Fill四连通域为0； 解2 若用并查集，则DFS(i,j,root)建立parent表（island中任意一点为root，不过还是得fill 1->0），find函数。这样可能再遍历1遍parent表，所以可能更复杂，不过作为一个思路。
* 系统中有很多个tag，tag间存在父子关系，如“iOS”和“Android”这两个tag都是“编程”这个tag的孩子。一篇文章会包含多个tag，给一个tag，输出包含这个tag以及其子tag的文章的列表。
  输入tag_record，这其中的每一项都是一个 (父tag，子tag) 的二元组
  输入tag_article，这其中的每一项都是一个 (文章tag，文章id)的二元组
  输入tag，即查找的tag
  输出符合的文章名列表
  解法：用字典和树，用字典实现tag -> tree node的查找。Tree的某个节点对应一个tag，节点保存了tag值，以及children这个属性，children存了所有直接children的tag（list[tag]）。一遍遍历tag_record后就维护了这个有父子关系的树结构，然后就可以很方便地查到输入tag以及它的子tag们，然后和tag_article（建立HashMap<String, Set(Id)>）中的记录比较来DFS查找即可。
* 找到粉丝数超过N（直接关注或间接关注）的blogger.（给定A关注B的信息）A：十分典型的并查集，需要find和HashMap<String, Set()>（defaultdict(set)）即可，如果嫌Set空间开销大，可以HashMap<String, Int>。`需问清楚"间接关注"是什么意思`

### maxStack
* [Min Stack](https://leetcode.com/problems/min-stack/)
  * 辅助Stack，same size as DataStack（询问Stack是否已经实现，还是要再实现）
  * 实现有getmax()函数的栈
  * 每次pop遍历取最大

### 树
* ++++[字典前缀树](https://leetcode.com/problems/implement-trie-prefix-tree/) Leetcode208。从root开始，当前letter作为key，找到它的children。work表示从root到当前node组成的word是否是insert。
```python
    def search(self, word):
        node = self.root
        for i in word:
            if i not in node.children:
                return False
            node = node.children[i]
        return node.word
```
* 平衡二叉树
  * [balanced binary tree](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#) +++ Leetcode110
    <del>*  实现一个depth递归获取当前节点的深度。isBalanced函数，递归检查是否平衡。复杂度o(nlogn)  </del>
    <del>* 实现一个depth递归获取当前节点的深度，并检查是否平衡，用全局变量self.isBalanced来标记。避免isBalanced再重复递归了O(logN) </del>
    * 思路：DFS post-order遍历，左右子树分别获得 Depth, isBalance，1）由此更新当前Node的深度 2）若左右子树任一不平衡，直接return False 3）或者说左右子树深度差 > 1 也return False
* ++有序数组构造平衡二叉树 LeetCode108，前序遍历。
```python
    def sortedArrayToBST(self, nums: 'List[int]') -> 'TreeNode':
        if len(nums) == 0:
            return None
        midIndex = len(nums) // 2
        root = TreeNode(nums[midIndex])
        root.left = self.sortedArrayToBST(nums[:midIndex])
        root.right = self.sortedArrayToBST(nums[midIndex+1:])
```
* ++有序链表构造平衡二叉树 A: LeetCode108，前序遍历。LeetCode109。`解法：与108思路一样，找中位数，然后DFS左右linked-list。区别在于维护的是node与length`
* 二叉查找树转成有序的双向链表 A: Leetcode426，中序遍历，用self.head记录新链表的头，用self.pre记录遍历时候的上一个node。
```python
        root.left = self.pre
        if not self.pre:
            self.head = root
        else:
            self.pre.right = root
        self.pre = root
```
* +++[验证二叉搜索树](https://leetcode.com/problems/validate-binary-search-tree/) 
    * 两个思路：1）中序遍历，看看前一个节点值是否小于当前节点值 2）根据当前子树的value range去解，`if node.val >= maxVal or node.val <= minVal: return False`，再用当前node.val设置value range， 继续dfs左右子树即可。
* 在BST中，[查找与目标值最接近的节点并返回](https://www.geeksforgeeks.org/find-closest-element-binary-search-tree/)。如果有多个节点都符合要求，返回其中一个即可。解法：从root开始遍历，记录目前为止minDiff与其为止。如果K>node.val，走右子树，若K<node.val，走左子树。记录最接近的，时间复杂度O(logN)
* 中序遍历树++ 解法：1）DFS 2）或者用Stack, `while root: stack.append(root)；root = root.left`
* 二叉搜索树 实现search node, insert node, delete node 每一个function都有问要用什么样的test case去测试，感觉有些面试官会比较看重这个。A: PyCharm426有实现insert。search就按照value comparison从root开始遍历到leaf。delete分3种情况（删root，删leaf，删中间节点）1.没左子树 2.没右子树 （前两者一样思路，比较简单，基本不用动树结构）3.有左右子树，用左子树往右走到底的value，或右子树往左走到底的value替代当前值。 [https://blog.csdn.net/zxnsirius/article/details/52131433](https://blog.csdn.net/zxnsirius/article/details/52131433)
* 最低公共父节点LCA（剑指offer） [Leetcode236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)
    1. BST，根据value大小的比较。if node.value > both(v1,v2) then 找左边 else 找右边，直到 v1<node.value<v2
    2. 有指向父节点的指针，转化为链表公共节点的问题
    3. 一颗普通的树。解法1）保存root->node的路径，转换为链表公共节点的问题。解法2）参考pycharm236 excellent，DFS后序遍历，从底往上，左右子树是否包含(p, q, None)，若是则返回。那么就会找到一个节点是p、q的最低公共父节点。
* +++[树的最大路径和](https://leetcode.com/problems/binary-tree-maximum-path-sum/) A：     [Accepted-short-solution-in-Java](https://leetcode.com/problems/binary-tree-maximum-path-sum/discuss/39775/Accepted-short-solution-in-Java) A: pycharm124有答案，思路，后续，`left = max(0, dfs(node.left))`
* ++[树的最长路径和](https://leetcode.com/problems/diameter-of-binary-tree/) `A:还是post-order，左右子树深度和 + 1视为diameter，递归时保存一个maxValue即可。`
    * follow-up [打印树的最长路径](https://www.geeksforgeeks.org/print-longest-leaf-leaf-path-binary-tree/) A：跟踪最长path，pycharm543有solution。
* 判断一个数组是不是二叉搜索树 剑指offer Q.33
  * follow-up: 后续遍历能用非递归写嘛？A：后序：左->右->根，可以把后序当作：根->右->左，然后再反转一下output list即可。 [https://blog.csdn.net/Mr_HHH/article/details/80212130](https://blog.csdn.net/Mr_HHH/article/details/80212130)
  * 给了一个数组，没给树，判断这个数组有没有可能是一个二叉搜索树后序遍历。A：1）数组最后一个是root； 2）从左到右根据大小关系找左子树subArray，找到分割的index 3）检查右subArray值是否小于root，若‘是’返回False；4）判断左右子树是不是，用递归。此时用subArray作为函数输入。5）return left and right
  

### 动态规划
* dfs leetcode221
  * [最大子矩阵](https://leetcode.com/problems/maximal-square/solution/)：最大正方形。在一个二维01矩阵中找到全为1的最大正方形。
  * [https://leetcode.com/problems/maximal-rectangle/](https://leetcode.com/problems/maximal-rectangle/) 很难，先skip
* +++卖股票[https://leetcode.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
  * follow up: 多次交易最大++ **(较难)**
* +++一个n \* m的棋盘上，起点在左上角，终点在右下角，每个位置上有不同的金额。只能向左边和下边两个方向移动，要求找到最大的金额以及路径。[minimum-path-sum](https://leetcode.com/problems/minimum-path-sum/)
    ```python
    # init
    for r in range(1, m): grid[r][0] += grid[r-1][0]
    for c in range(1, n): grid[0][c] += grid[0][c-1]
    # core
    for r in range(1,m):
        for c in range(1,n):
            grid[r][c] += min(grid[r-1][c], grid[r][c-1])
    ```
* [https://leetcode.com/problems/path-sum/](https://leetcode.com/problems/path-sum/) DFS
  [https://leetcode.com/problems/path-sum-ii/](https://leetcode.com/problems/path-sum-ii/) DFS + list
  [https://leetcode.com/problems/path-sum-iii/](https://leetcode.com/problems/path-sum-iii/) DFS + HashMap(to store all sums (root->predecessor) of all of this node's predecessors). if 
  ```python
  complementary = so_far + node.val - target # sum(root->node) - target is what we need in the previous path
  if complementary in cache:
      self.result += cache[complementary]
  ```
  
### 贪心

* 第一道题是已知n个节目的开始结束的时间，问最多能看多少节目。当然我知道这个就是个O(n)的贪心 A:以结束时间（升序）排序，找最早结束的（因为留下了最多的时间给后面节目），然后循环。
* Leetcode435 [https://leetcode.com/problems/non-overlapping-intervals/](https://leetcode.com/problems/non-overlapping-intervals/) 思路：Which interval would be the best first (leftmost) interval to keep? One that ends first, as it leaves the most room for the rest. So take one with smallest end, remove all the bad ones overlapping it, and repeat (taking the one with smallest end of the remaining ones).
* Leetcode56 [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/) 思路：以开始时间（升序）排序，当前访问的interval 是否 overlap （已经merge的）上一个日程`if out and i.start <= out[-1].end: out[-1].end = max(out[-1].end, i.end)`，是的话merge进去


### √two pointers

* 判断链表是否有环
  * 141. Linked List Cycle [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)
  * `判断两个链表是否相交` 是 `求链表是否存在环` 的 变式，如给定两个链表A和B
    * 思路1：A遍历自己，一直到终点再从B开始。B也如此，遍历完自己再遍历A; 如果中间有相同节点了，那么就相交，否则不相交; 因为如果相交的话，那么都会走到len\(A\)+len\(B\)-len\(A∩B\)的地方停下来
    * 思路2：分别遍历求出长度L1,L2，长的那个链表指针先走abs(L1-L2)步，再一起走，判断是否有相同node
* 判断链表是否有环，并找到入口节点
  * 142. Linked List Cycle [https://leetcode.com/problems/linked-list-cycle-ii/](https://leetcode.com/problems/linked-list-cycle-ii/)
  * 剑指offer有。思路：快慢指针先找到相遇的node，然后新开一个指针ptr3，与慢指针同速度走，相遇节点为circle入口
    ```text
     证明：
     慢ptr1：k路程，快ptr2：2k路程
     相遇时，k = 2k-k = bn（b圈，一圈n长度）
     bn + △（相遇点，再走到entry的距离）= bn + s（从头开始走，走完n圈），这两者路程相等 => △ = s
    ```
<del>* 给一个链表头，返回是单链表还是环链表，然后再补一个给一个链表头返回长度的。</del>

### 大数据：

* 数据流中位数[https://leetcode.com/problems/find-median-from-data-stream/](https://leetcode.com/problems/find-median-from-data-stream/) `A: 最大堆，最小堆。`
    * follow-up: 如何用dp优化？
* [十亿数排序](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#) [总结](https://legacy.gitbook.com/book/lbgzmhl/microsoft-suzhou-interview/edit#)
* 拓扑结构

## 逻辑题

### √ 概率题（用少表示多并均匀）

* 给一串只包含0~9的数字串，每个数字出现的概率相同（比如32978417506），现在告诉你（1,3,5,7）这四个数字不可用，即只能用（0,2,4,6,8,9）这6个数，如何表示原数字串？
  * 00表示0, 02表示1, 04表示3， 06表示5， 08表示7
  * 15位表示以前的10位，所以存储多出来0.5倍

### √ 凸包问题

#### 求包裹所有点的凸多边形

* 穷举法O\(n³\)
  * 思路：两点确定一条直线，如果剩余的其他点都在这条直线的一侧，则这两个点是凸包上的点，否则就不是。
  * 步骤：1.将点集里面的所有点两两配对，组成 n\(n-1\)/2 条直线。2.对于每条直线，再检查剩余的n-2个点是否在直线的同一侧。
  * 结果为正时则\($$x_3, y_3$$\)在直线左侧，为负在右侧

![&#x4E09;&#x70B9;&#x8DDD;&#x79BB;&#x516C;&#x5F0F;](.gitbook/assets/image%20%289%29.png)

* 分治法O\(nlogn\)
  * 思路：找到凸包点，分割点集，在子点集上继续找凸包点
  * 步骤：
    * 找到横坐标最大的P1和P2肯定是凸包点，分割为上下包
    * 对上包，求**距离直线最远的点（公式如上）**，即图中Pmax
    * 把P1Pmax的左侧和PnPmax的右侧当上包
    * 重复第二三步，对下包也这样

![](https://lbgzmhl.gitbooks.io/microsoft-suzhou-interview/content/assets/%E5%87%B8%E5%8C%85.png)

* 步进法O\(nH\)
  * H是闭包点的个数。
  * 思路：
    * 先找纵坐标最小的点加入闭包
    * 从P0开始，逆时针找点集上使alpha角\(和x轴夹角\)最小的点，即P1
    * 从P1出发，逆时针找点集上使alpha角\(P0P1和P1P?夹角\)最小的点，即P2
    * 重复，直到回到P0。注意共线时点都加入闭包，但选最远点为下一次的出发点。

![](https://lbgzmhl.gitbooks.io/microsoft-suzhou-interview/content/assets/%E6%AD%A5%E8%BF%9B%E6%B3%95.png)

* Graham扫描法O\(nlogn\)

![](https://lbgzmhl.gitbooks.io/microsoft-suzhou-interview/content/assets/20150530145453912.gif)


## 补充
## **微软十五道面试题**

1. 有一个整数数组，请求出两两之差绝对值最小的值。 记住，只要得出最小值即可，不需要求出是哪两个数。

2. 写一个函数，检查字符是否是整数，如果是，返回其整数值。（或者：怎样只用 4 行代码编写出一个从字符串到长整形的函数？）

3. 给出一个函数来输出一个字符串的所有排列。

4. （a）请编写实现 malloc()内存分配函数功能一样的代码。 （b）给出一个函数来复制两个字符串 A 和 B。字符串 A 的后几个字节和字符串 B 的前几个字节重叠。

5. 怎样编写一个程序，把一个有序整数数组放到二叉树中？

6. 怎样从顶部开始逐层打印二叉树结点数据？请编程。

7. 怎样把一个链表掉个顺序（也就是反序，注意链表的边界条件并考虑空链表）？

8. 请编写能直接实现 int atoi(const char * pstr)函数功能的代码。

9. 编程实现两个正整数的除法，当然不能用除法操作符。
// return x/y.
int div(const int x, const int y)
{
....
}

10. 在排序数组中，找出给定数字的出现次数。 比如 [1, 2, 2, 2, 3] 中 2 的出现次数是 3 次。

11. 平面上 N 个点，每两个点都确定一条直线，求出斜率最大的那条直线所通过的两个点（斜率不存在的情况不考虑）。时间效率越高越好。

12. 一个整数数列，元素取值可能是 0~65535 中的任意一个数，相同数值不会重复出现。
0 是例外，可以反复出现。
请设计一个算法，当你从该数列中随意选取 5 个数值，判断这 5 个数值是否连续相邻。
注意：5 个数值允许是乱序的。比如： 8 7 5 0 6
0 可以通配任意数值。比如：8 7 5 0 6 中的 0 可以通配成 9 或者 4 
0 可以多次出现。复杂度如果是 O(n2)则不得分。

13. 设计一个算法，找出二叉树上任意两个结点的最近共同父结点。

14. 一棵排序二叉树，令 f=(最大值+最小值)/2，
设计一个算法，找出距离 f 值最近、大于 f 值的结点。
复杂度如果是 O(n2)则不得分。

15. 一个整数数列，元素取值可能是 1~N（N 是一个较大的正整数）中的任意一个数，相
同数值不会重复出现。
设计一个算法，找出数列中符合条件的数对的个数，满足数对中两数的和等于 N+1。
复杂度最好是 O(n)，如果是 O(n2)则不得分。