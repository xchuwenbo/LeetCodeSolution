- **Binary Tree Postorder Traversal (145)**
	- 要求不用递归实现后序遍历
	- 后序是left-right-root，那么首先用修正的前序root-right-left，然后reverse一下，变成left-right-root就行了，代码如下：

- **Factorial Trailing Zeroes(172)**
	- 给出整数n，求$n!$末尾的0的个数，要求在logarithmic时间内求解
	- 首先容易知道要出现0，只有$2 \times 5 =10$才行，而对于从$1,2,...,n$的这一系列的数，是2的倍数的数明显比5多，因此每一个5一定能找到一个2与之配对，换句话说，0的个数和5的个数一样多，下面只需要找出5的个数就行了，容易知道反复除以5就行，代码如下：

- **Container with most water(11)**
	- 给出一些列的隔板的高度，求问用哪两个隔板装水能装最多的水
	- 求解非常巧妙，左右分别向中间靠拢，指针记为left和right, 如果left的值比right的小，则left往中间移动一格，否则right往中间移动一格

- **Maximal Rectangle(85)**
	- 题目的要求是给出一个0-1矩阵，要求给出一个最大的矩形，使得矩形里面的所有值为1
	- 解法非常巧妙，从上往下扫描，维护三个数组**height**, **left**, **right**，分别存储每一列的信息，当迭代到第i行的时候，left表示从左往右最靠右的0的位置，初始为0；right表示从右往左最靠左的0的位置，初始为cLen+1，因此列的编码是(0, 1, 2, ..., cLen, cLen+1)，然后height存的就是当前单元从上往下连续的1的个数
	- 维护好了这些信息，那么每个单元格的面积就是
$$Area(j) = height(j)*(right(j) - left(j) - 1)$$
	- 值得注意的是，在更新left和right的时候，得查看上一迭代的时候height的值，如果是0，那么本层不受约束，如果是1，那么考虑到上下直接的连通，left的值不能小于旧的left值，right的值不能大于旧的right值

- **Regular Expression Matching(10)**
	- 这题难度比较大，就是普通的正则表达式匹配问题，即`*`和`.`，前者表示`*`前面的字符出现任意次，后者可以替代任何字符。
	- 用动态规划的思路做，设一个二维矩阵b[m+1][n+1]，其中m是string串的长度，n是pattern串的长度，然后b[i+1][j+1]代表的意义就是s[1..i]和p[1..j]是否能匹配上
	- 有了上述的约定，首先置b[0][0]=true，然后再初始化第一行和第一第二列，接着利用递推关系进行求解。详细的递推请参考这儿[猛戳我](http://xiaohuiliucuriosity.blogspot.com/2014/12/regular-expression-matching.html)

- **Wildcard Matching(44)**
	- 这题相对于上面的正则匹配容易很多，只是通配符`*`和`?`匹配，但是leetcode平台对这题进行了专门的限制，最后给出了一个超长的测例，最终会RunTime Error。因此需要一种改进的贪心算法，非常高效，大致思想是如果遇到了`*`，那么先暂时记录位置，只使用一次，如果发现不行就再多使用一次

- **Binary Search Tree Iterator(173)**
	- 题目意思非常清楚，在O(1)的**平均**时间复杂度和O(h)的空间复杂度下从小到大输出一个二叉查找树
	- 这题的难点就在于要求O(h)的空间复杂度，因为直接能想到的方法是利用递归算法中序遍历一棵二叉查找树，并且用一个queue存起来。然后再依次输出，但是这样的空间复杂度是O(n)，不符合要求，因此需要另谋出路
	- 一种方法是维护两个栈path和aux，其中aux记录当前深度搜索过程中的所有节点。当要求输出下一个的元素的时候，只需要进入一个循环，循环内部每次从path中取出一个节点current，和aux的`top()`节点比，如果不相等，说明还没有到树的末端，于是依次把current的右节点，current本身和current的左节点压入path, 同时需要把current本身压入aux，表示current节点是当前深度优先搜索路径上的点。接着如果两个栈的`top()`元素相等，那好办，两个都弹出顶端节点，然后返回其值。
	- 另一种更加省空间的方法是，维护一个栈path，并且定义一个`pushAll`的函数，该函数的功能是把给定节点的所有左节点压入path。每次要求输出下一个节点的时候，取出path顶端的节点current，然后把调用`pushAll(current.right)`，并且返回current.val。
	- 以上两种方法，经过测试发现第二种更快，需要32ms，第二种仅仅需要31ms

- **Intersection of Two Linked Lists(160)**
	- 题目是在一个Y型的链表中，已知两个头指针的位置，问交汇的地方在哪儿，如果没有交汇点，则返回NULL，并且在程序返回的时候不能改变链表的结构，时间复杂度要求$O(n)$，空间复杂度要求$O(1)$
	- 这题虽然是easy题，但是思路非常难想到，一般都会想着如何标记信息，但是这题恰恰把这一条路给堵死。其实仔细想想，如果想要两个点相遇，只需要走的长度一样就行，这样我们就可以让首先达到终点的指针从**另一条链**从头开始，后到达的指针也从另一条链开始，这样两个指针将会同时到达交汇点

- **Implement strStr() (28)**
	- 题目要求给定两个字符串haystack和needle，找出haystack中第一次匹配上needle的下标，如果不存在，则直接返回-1
	- 这道题可以直接进行线性搜索，当needle的某一次匹配不成功的时候，需要回退到匹配上第一个字符时候的下标，并且加1重新开始匹配needle。直到达到末尾，或者匹配成功

- **Path Sum II(113)**
	- 这题其实题意就是找出所有从根节点到叶子节点的路径，使得路径上的节点值的和为给定的值。题目本来并不难，只需要深度优先遍历即可，但是坑就坑在有负值，因此一开始本来用栈+剪枝的方法，后来发现不好使，各种bug，还是乖乖用递归解决。

- **Word Ladder(127)**
	- 这题非常有趣，给出start word和end word，以及一个字典，要求每次变换一个字符，使得start word能转变成end word，并且变换中间的word都必须在字典中。这个最暴力的方法就是首先构造一个图，节点是合法的word，边是edit distance正好为1。然后在图上使用BFS就行了。但是这样容易LTE，因此思路需要转变
	- 一种可行的方法是从start word角度出发，对所有的可能做遍历，然后用字典进行筛选剪枝，每当发现一种合法的变换(必然是字典中的word)，那么立即把新word从字典中删除，并且push进队列。如此循环，直到到达end word。

- **Permutations I(46)**
	- 生成全排列，并且没有重复元素。这个比较好办，可以使用递归式，即
	$$P(a_1,...,a_n)=\{a_1, P(a_2,...,a_n)\} \cup ... \cup \{a_n, P(a_1,..., a_{n-1} \}$$
	- 设计函数`void recursion(int []num, int n, int len)`，其中n为遍历的位置，如果`n==len-1`，函数返回

- **Permutations II(47)**
	- 这题允许集合中出现重复，因此主要任务是如何去重。容易知道假设目前待交换的位置为`i`，并且此时正在`i<=j <=len-1`中对`j`进行遍历。在46题中，直接交换`num[i]`和`num[j]`是没有任何问题的，因为没有重复，但是在本题中首先如果`num[i]==num[j]`，则必然会产生重复；除此以外，如果存在`i<=k<=j-1`使得`num[k] == num[j]`，那么说明在遍历到`j`之前已经替换过和`num[j]`相等的`num[k]`了，这个时候也会产生重复。
	- 综上知道，只需要保证在`i<=k<=j-1`中没有元素满足`num[k]==num[j]`即可

- **Balanced Binary Tree(110)**
	- 这题本来非常简单，但是一开始概念混淆了，认为**平衡**的意思是对于每个节点，两棵子树的最大最小高度之差不能大于1，这样在递归调用的时候就得同时返回最大值和最小值，非常麻烦。但实际上的定义是高度不大于1即可，也就是最深的路径之间大小不大于1即可，这样在递归调用中就仅需要返回最大值即可。

- **Surrounded Regions(130)**
	- 这题类似围棋，给定一个width*height的棋盘，上面有`X`和`O`两种棋子，如果一个`O`的四个方向都被`X`包围，那么`O`变成`X`
	- 这题的思路就是找找到和边界相连通的所有`O`，然后把非连通的`O`给设置为`X`即可。具体实现方法是维护`bool`型的数组`visited`记录是否已经访问，并且维护一个队列，存放当前新发现的连通的`O`，这样，每次取出一个`O`，检查其**前后左右**四个方位的`O`棋子，如果未访问，则塞进队列。直到队列为空。

- **Palindrome Partition II(132)**
	- 这题就是找出给定字符串`s`中最小的切割次数，使得切出的字串都是回文。
	- 解题思路是用动态规划做，最快的方法是维护一个整型数组`mincut`，其中`mincut[i]`保存的是以`i`截止的最小切割数。更新过程，分为偶数更新和奇数更新
		- 奇数更新就是以下标`j`从头开始遍历，比较`i-j`和`i+j`的值，如果相等继续向两边扩展，并且令`cut[i+j+1] = min(cut[i+j+1],1+cut[i-j])`
		- 偶数更新同样以下标`j`从头开始遍历，比较`i-j+1`和`i+j`的值，如果相等继续向两边扩展，并且令`cut[i+j+1] = min(cut[i+j+1],1+cut[i-j+1])`
	- 非常简洁高效，当然，不是我想出来的....

- **Merge k Sorted Lists(23)**
	- 这题是**Merge Two Sorted Lists**的加强版，就是给定n个sorted lists，然后将他们串起来组成更长的sorted list。最直接的方法就是用优先队列，利用小根堆或者大根堆，但是发现效果不佳，61ms，可能是实现的原因。在网上一找发现更加高效的办法，而且不需要用优先队列，利用的就是两两合并的方法。具体来说，首先两两合并，然后把数量已经减半的lists继续两两合并，直到只剩下一个list。
	- 看起来遍历了很多遍，但是这种方法一个是实现起来快速，另一个隐含的加速的地方就是如果两两合并的时候某一个链表到了末端，那么只需要另一个链表把剩下的部分直接接到目标链表末端即可。

- **valid sudoku(36)**
	- 题目要求检查已给给定的数独是否是合理的，所谓合理就是每一行数字不同，每一列数字不同，每一个9宫格数字各不同。最直接的暴力方法是扫描，这样需要扫描的次数为$9*9*3=243$次，每次扫描用一个`unordered_set`来存储所有的遇到的数字，如果有重复立即返回false。
	- 另外一种非常快的方法是三种情况同时扫描，这样只需要对每个单元格访问一次即可，$9*9=81$次。此外，受到**基数排序**的启发，新建三个boolean型$9*9$的数组，每一列分别存储上述三种情况遇到的数字，由于数字范围是1-9，正好可以映射到下标0-8，一旦映射后发现数组对应单元格已经是true，说明产生了冲突。

- **Spiral Matrix I(54)**
	- 这题非常有意思，就是给定一个$m \times n$的矩阵，要求从最左上角开始，绕着矩阵顺时针依次打印，不能重复已经访问的元素，相当于绕一个内螺旋。大致的思路都是一样的，每次递减边界即可。但是实现上面有很多优化的地方，例如可以设置一个二维数组`int ops[4][2] = {{0,1}, {1,0}, {0,-1}, {-1,0}}`，这样每次就只需要做选择即可。

- **Spiral Matrix II(59)**
	- 属于54题的加强版，这次的输入是一个整数n，要求返回$n \times n$的方阵，其中方阵按螺旋顺序依次为$1, 2, ..., n^2$。这题思路其实和上题一致，但是不同的地方是首先得初始化矩阵，这样就方便后面的随机访问。

- **Subsets I(78)**
	- 给定一个没有重复的集合，求其所有的子集，并且打印的时候子集中的序列是单调递增的。显然有$2^n$个，循环生成即可，首先对给定集合排序，每次取一个元素，然后将原来的数组个数倍增，然后在新增的数组里里面添加新的元素即可。如此循环，直到到达末尾。

- **Subsets II(90)**
	- 这题是78题的加强版，不同之处是允许给定的集合有重复元素，这个时候思路和上述差不多，但是在对原始的所有数组进行遍历的时候，如果当前数组的最后一个元素和将要添加的元素一致，那么就果断直接添加，不进行扩增；如果不一致，那么策略与78一样，生成一个一模一样的数组，然后在新的数组末尾添加上新的元素。

- **Candy (135)**
	- 这题是分糖果的问题，一群小朋友排成一列，每个人有个rating值，要求rating值大的分的糖果要比**身边的孩子**多，注意没有约束如果rating值相同的情况，因此可以任意分配。题目的最终要求是以最少的糖果数满足所有的孩子，并且每个孩子至少分得一个糖果。
	- 一种非常简单的思路就是扫描三遍，首先从左往右扫描，处理好递增的情况；然后从右往左扫描，处理好递减的情况；最后遍历一遍数组把所有的值相加即可。这样一共需要扫描3遍，但是复杂度仍然是$\Theta(N)$
	- 另一种的思路就是发现，如果出现连续相同的rating，如$2,2,2,2$，那么实际上，我们可以拆分为三部分$..., 2 | 2, 2 | 2, ...$，中间的由于没有约束，因此全部取成1，然后左右两个子问题相互不相关，可以分别处理，并且，这两个子问题里面是肯定没有连续重复的rating的。因此，这种方法一共需要扫描2遍数组，第一遍拆分，第二遍对每个子问题求解。而每个子问题求解的方法就是贪心算法，具体参加代码

- **Valid number(65)**
	- 这题非常蛋疼，就是判断一个给定的字符串是否为合法的数字，包括各种奇葩情况。主要的解题思路就是用**有限状态机即可**，效率最高，唯一可以精简的就是状态的数量。详细请看代码

- **Reverse Integer(7)**
	- 这题要求把给定的32位有符号的整数进行反转输出，如`-123`输出`-321`，这里的关键是判断是否溢出，如`1000000003`反转后是会溢出的，因此需要找到一种高效判断溢出的方式。
	- 首先每次的更新是取`x`的余数`remainder`，然后`newReverse = 10 * reverse + remainder`进行更新即可，判断是否溢出可以通过简单的`(newReverse-remainder)/10==reverse`进行判断
	- 外层循环的退出条件是发现`x==0`

- **Reverse Bits(190)**
	- 这题和7有点类似，但是不同之处是整数变成了无符号数，并且要求按位对这32bits进行反转，因此外层循环必须要执行32次才行，因为2进制数可能高位为0。直接的方法是把7直接改一改，就可以通过测例。
	- 但是有一种更为巧妙的方法，例如对`AB`进行反转，相当于先变换成`BA`，然后再依次对`B`和`A`内部进行反转，即$(AB)^T=B^TA^T$。而此时`B`和`A`变成了原始问题的子问题，规模缩小，于是利用分而治之的办法便可以各个击破。效率惊人，非常快，8ms，而之前方法需要10ms。

- **Min Stack(155)**
	- 这题要求实现一个栈，但是和普通的栈相比多了一个操作，返回当前栈中的最小元素。一开始想到的办法是维护两个栈，一个存数据`stack_data`，一个存当前的最小值`stack_min`，每`push()`一个新的值`x`，把`min(x, stack_min.top())`重新压入`stack_min`，每次`pop()`的话两个栈同步进行弹栈。这样做固然可以，但是17个测例的成绩是80ms，非常慢。
	- 后来仔细一想，其实没必要每来一个新元素就同步压栈，而是发现`x`**不比当前**`stack_min.top()`的值大的时候再压入`stack_min`，即`x<=stack_min.top()`。这样就能保证`stack_min`中的值是**不增**的，并且元素个数比以前的方法也小了很多，最后的成绩是66ms

- **Roman to Integer(13)**
	- 这题是12题的姊妹题，要求给定一个罗马数字字符串，转换为相应的十进制数字。思路并不难，设置变量`previous_value`, `current_value`和`total`。从左往右扫描
		- 如果`current_value <= previous_value`，那么直接`total+=current_value`
		- 否则，`total+=current_value - 2*previous_value`即可。
	- $O(n)$的复杂度

- **Number of 1 Bits(191)**
	- 题目本身不难，就是给定一个无符号的32位整数，统计1的个数。
		- 最直接的方法就是逐位统计，只需要32次循环即可
		- 稍微改进的方法是遍历的时候留意原始值的范围，例如输入的是`3`，那么只需要判断2位即可
		- 最变态的方法是利用位运算，思路是首先以2位为一组相加，然后4位位一组相加，以此类推，最后16位为一组相加，得到的值就是输出值。

- **Insert Interval(57)**
	- 题目给一系列不重合的区间，并且给定一个新的区间，要求将新的区间插入给定的一系列区间中，必要的时候进行区间的合并。
	- 求解思路其实不难，就是首先找出所有和新区间不相交的区间，依次加入到新的区间数组中；然后把与新区间相交的区间进行合并，并把合并后的结果插入新的区间数组中；最后把剩余的区间插入新的区间数组中即可。

- **Scramble String(87)**
	- 这题给定一个字符串，容易知道可以用一棵树来表示这棵字符串，对于任意一个非叶子节点，交换其两个结点可以得到新的字符串，那么称新得到的字符串为原始字符串的Scramble String。那么现在的问题是，如果给定两个字符串，判断他们是否互为Scramble String。
	- 暴力搜索的办法就是每次比较两个字符串中字符的频率数，如果相等则认为有可能是Scramble String；接着遍历所有的切分位置，分别将两个字符串切成两段**(s11, s12)**和**(s21, s22)**。递归判`isScramble(s11, s21) && isScramble(s12, s22)`和`isScramble(s11,s22) && isScramble(s12, s21)`，其中只要有一种情况值是真，那么原始的`isScramble(s1,s2)`就是真，否则返回假。这种方法的复杂度其实非常高，但是实际的效果却出奇的好
	- 第二种方法是三维动态规划，复杂度是$O(n^3)$，维护的主要是`res[i][j][n]`，代表的意思是s1和s2的起始位置分别是i和j, 然后长度是n, 判断这两个字符串是否是isScramble。这样就可以利用更短的已经解决的问题去求解更加长的问题。最后返回`res[0][0][len]`

- **Binary Tree maximum path sum(124)**
	- 这题颇有意思，是给定一棵二叉树，然后返回所有从结点到结点的路径中最大的路径和值，注意是有负数的，直观上肯定需要用递归，然后可以扫描一次解决问题。关键就是如何设计递归的返回值。
	- 从最优解出发思考，最优解的组成要么就是所有点都在不同的层，即自上而下一条链，要么就是存在两个结点在同一层，即出现一个折线，无论是哪一种情况，总可以找到这个最优解的根。这里所谓的根就是链离树根最近的结点或者折线的拐点。这样，我们在搜索树的时候，就可以把当前遍历的点看做根，然后比较**经过根的路径**和**不经过根的路径**，而容易知道在当前结点看来**不经过根的路径**肯定是更低层的某个结点的**经过根的路径**，因此问题转换为找出所有经过根的路径的最大值。
	- 据此，设计了递归函数`max_to_bottom(TreeNode *current_node, int &max_seen_value)`。当递归到某个结点时，分别对其左右子树调用`max_to_bottom`，然后计算`go_through_value=left+current+right`计算出经过当前结点的最优路径，并且和`max_seen_value`比较并对`max_seen_value`进行更新。
	- 这样，就只需要扫描一遍树，树的根节点只需要取当前结点的`go_through_value`和`max_seen_value`中的较大值返回，问题即可得到解决。
	- 实现的细节请参看源码

- **String to Integer(8)**
	- 这题就是实现**aoti**，有许多细节需要注意，第一个就是开头有空白，第二个就是可能有符号位，接着就是字符串一旦出现了除了0-9之外的数字，那么认为截断，最后需要妥当处理的就是溢出问题了。
	- 解题思路是首先去掉所有的空白，然后记录符号位，接着利用`y = 10 * x + c`的方法更新，对于溢出的处理，目前所知最精简的处理方法就是` x > INT_MAX/10 || (x == INT_MAX/10 && c > 7)`。最后根据符号位进行适当的返回即可。

- **Merge Sorted Array(88)**
	- 这题就是要把两个排好序的数组合并成更大的数组，其实思路很简单，不需要从左往右进行排序，而是**从右往左**，扫描一遍即可，详细请看代码

- **Interleaving String(42)**
	- 这题给定三个字符串**s1**, **s2**和**s3**，问s3能否通过s1和s2交叉组合在一起，这里所谓交叉组合就是两个串的字符进行交错插入，生成新的长度为**len1+len2**的串。方法是用动态规划解决，利用2维表格，里面的每一个元素**(i,j)**代表s1的前i个字符和s2的前j个字符能否交叉组合成s3的前i+j个字符。由此不难看出，(i,j)为`true`有两种情况
		- 一种是相邻的同行左端元素为`true`，并且s2的**第j个**字符和s3的**第i+j**个字符一致
		- 另一种是相邻的同列上端的元素为`true`，并且s1的**第i个**字符和s3的**第i+j**个字符一致
	- 最终返回**(len1, len2)**即可，注意表格维度为`int[len1+1][len2+1]`，下标0代表空串
	- 由上述的动态规划我们得到一个重要的观察，就是只要存在一条通路从左上角到达右下角，那么原问题就是`true`，于是我们可以转换思路用**DFS来求解这道问题**。同样需要设置一个二维数组，这个时候0代表还未访问，1代表已经访问，从左上角开始分别向下和向右探索，倘若**下一个单元格没有访问并且可行**，那么递归调用下一个单元。任何时候到达了右下角，立即返回。
	- 上述DP和DFS算法的最坏复杂度一样，最后大概都是在5ms左右。理论上DFS会有微弱优势，因为DFS可能不需要访问所有单元格就可能找到解，而DP必须访问完所有单元格。

- **Trapping Rain Water(42)**
	- 这题非常形象，就是给定一个数组代表隔板的高度，每个隔板宽度都是1，问如果天上下雨的话，这个隔板系统最多能装多少雨。分析这个问题，对于每一个格子，究竟能装多少雨其实取决于这个格子**左边最高隔板**和**右边最高隔板**，其中比较小的隔板就是最终当前格子能装雨的最大高度。
	- 有了上述的重要观测，就可以设计一个朴素的**时间和空间都是O(n)**的算法了，设置一个`int`数组`peaks`，从左往右扫描一次，更新遇到的最大值，并用这个最大值填充相应的元素；然后从右往左也扫描一个更新`peaks`，这个时候`peaks`存的就是每个单元格理论上能装的最大高度的水，然后在逐个做差累加即可。
	- 除了上述算法，还有一个加强版算法**时间O(n), 空间O(1)**，主要思路是分别从两端向中间靠拢，假设i和j分别是左右两端的下标，选取`height[i]`和`height[j]`中比较小的进行更新，直到相遇。详细过程请参看代码。

- **Excel Sheet Column Title(168)**
	- 这题非常奇葩，引入了一套不含有0的计数系统，应用背景是Excel的编号，如`A-1`, `B-2`，....， `Z-26`，`AA-27`等等，要求把一个整数转换为字母表示的形式。由于不含有0，因此不能按常规的转换，不同的地方是每次`r = n % 26`和`n = n / 26`后，得判断`r`的取值，如果为0，说明除尽了，那么`n = n - 1`，否则按正常的`r + 'A' - 1`

- **Linked List Cycle II(142)**
	- 这题是典型的快慢指针问题，给一个链表，检测上面是否有环，如果有的话返回环开始的地方，否则返回`NULL`。只需要维护两个指针，慢指针一次前进1个单元，快指针一次前进2个单元，假设在**ptr**相遇，这个时候只需要重新设定两个指针分别从**ptr**和**head**同步前进，相遇的地方就是环开始的地方。

- **First Missing Positive(41)**
	- 这题简直太巧妙了，给定一个无序的整型数组，要求返回第一个缺失的正整数，并行时间复杂度要求$O(n)$，空间复杂度要求$O(1)$
	- 这题有个非常重要的观察，就是无论整数的范围如何，第一个缺失的正整数最大不超过$n+1$，其中$n$为元素的个数。有了这个重要的观察，那么我们就可以考虑把所有大于0并且不超过n的元素按次序放到对应的单元里面，这需要一次扫描。接着进行第二次扫描，从左往右，找出第一个错位的元素即可。

- **Reverse Linked List II (92)**
	- 这题要求把一个给定的链表从指定开始位置`m`到结束位置`n`翻转。一般来说需要特殊考虑`m=1`的情况，但是发现有种非常hack的方法是设置一个**虚拟**指针指向开头，有了这个作为保证，就可以不断地把`AB`交换为`BA`延伸下去，如`ABCD->BACD->CBAD->DCBA`，无需考虑边界条件，非常高效。

- **N-Queens II(52)**
	- 著名的N皇后问题，常规解法是**回溯法**，即深度优先搜索加上剪枝，值得注意的是实现上的一些小tricks，就是棋盘可以用一维数组表示，然后判断是否在对角线上可以用`abs(i-j) == abs(q[i] -q[j])`来快速判断
	- 后来上网一搜才发现还有更加变态的解法，就是用比特位与异或运算来求解，非常的巧妙，设置三个变量`row`，`left`和`right`，分别表示由于同一列，左对角线和右对角线的**禁位**，只要`row|left|right`就是所有的禁位，详细解法请参看源代码

- **Jump Game(55)**
	- 题意很简单，就是给定一个整型数组，表示每个元素能往前前进的最大步长，然后问是否可以从第一个元素开始到达最后一个元素。这题容易想复杂，其实只需要最远能到达末端元素即可，按照这样的思路，只需要维护一个变量`farthest`，表示目前已知的最远距离，然后扫描一遍即可，不断用`farthest = max(farthest, i+jump[i])`更新即可

- **Edit Distance(72)**
	- 编辑距离是一道典型的DP问题，就是求两个字符串`s1`和`s2`的最少编辑次数，包括删除，插入和替换
		- `s1`增加一个字符后和`s2`匹配
		- `s1`删除一个字符和`s2`匹配
		- `s1`和`s2`各删除一个字符然后观察最后一个字符是否可以进行替换
	- 于是我们可以得到公式`E(m, n)=min{1+E(i-1, j), 1+E(i, j-1), diff(i, j)+E(i-1, j-1)}` 

- **Recover Binary Search Tree(99)**
	- 这题是将某个二叉搜索树的两个节点交换了位置，要求找出这两个节点。看似非常复杂，其实想明白后，就可以利用**中序遍历**的思想，如果在遍历过程中发现前一个节点比当前节点的值大，那么说明存在错位。这里关键需要分清楚的是有两种情况：
		- **相邻**，这个时候只需要交换这一前一后两个节点即可
		- **不相邻**， 这个时候交换错位的节点对的前一个节点，如`12345`变成了`14325`，错位的有`43`和`25`，此时交换`4`和`2`即可。

- **Partition List(86)**
	- 给定一个链表，如`1->4->3->2->5->2`，给定一个整数，如`x=3`，要求将链表按整数进行划分，并且保证相对位置，结果为`1->2->2->4->3->5`。
	- 解决方法是设置两个虚拟指针分别存储**大于x**和**不大于x**的元素，最后将其串起来即可

- **Maximum Gap(164)**
	- 给定一个无序的数组，要求找出按序拍好后的**最大间隔**
	- 有个非常重要的观察就是倘若数组的最小最大元素为`min`和`max`，数组长度为`len`，那么最大间隔至少为$bucketSize=\lceil (max-min) / (len-1) \rceil$，这样就可以按bucketSize的大小将元素划分若干bucket，个数为$bucketNum=\lceil (max-min)/bucketSize\rceil$。接着将所有元素映射到相应的桶内，存储每个桶内的最大值和最小值，然后扫描一遍所有桶，找出桶与桶之间的最大值即可。
