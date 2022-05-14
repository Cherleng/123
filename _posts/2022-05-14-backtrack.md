### Backtrack 算法思路



#### 1.引言

>黄色的树林里分出两条路，可惜我不能同时去涉足,我选择了人迹稀少的一条，从此决定了我一生的道路

#### 2.什么是回溯算法？


Backtrack算法，中译回溯算法，是一个非常实用的一个算法，如果用一个谚语来形容回溯算法，我愿意称之为“不撞南墙不回头”的一种算法。


为什么这么说呢？因为回溯算法实质上就是一种不断尝试，通过不断的“试错”不断更改信息的一种算法。


“不撞南墙”说明了当回溯算法不遇到错误的时候，“不回头”就是不返回上一步重新尝试。

从引言的那首罗伯特.弗罗斯特的诗里，“可惜我不能同时去涉足”，可是对于回溯算法来说不是这样的，回溯算法看到路走错了就不会继续走了，它会折返到最近的一个分岔口，重新选择一条道路行走。

![分岔的道路](https://tse1-mm.cn.bing.net/th/id/R-C.91867e276501f596a4802b67aea756f6?rik=T2H1qdJZGAQe7g&riu=http%3a%2f%2fimg95.699pic.com%2fphoto%2f50123%2f6755.jpg_wh300.jpg!%2ffh%2f300%2fquality%2f90&ehk=6ZJzrS%2bT3A%2fCApII9W0nWQRrSgaCGaU1C3o6wI7aRLs%3d&risl=&pid=ImgRaw&r=0&sres=1&sresct=1)


回溯算法可以用来解数独，进行排列组合，解决N皇后等问题



#### 3.回溯算法的实现

让我们来看一看[79. 单词搜索](https://leetcode.cn/problems/word-search/) 这道题

这道题要求我们从board里面查找某一个单词（例如：ABCDE)，如果查找到单词，我们就返回True，否则返回False。

![单词搜索](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)



如上所示，黄色的路径就代表我们成功找到了“ABCDE”这个连续的单词。我们可以从上、下、左、右四个方向进行单词查找。

这里有一点需要注意，单词并不需要第一行第一列的数字来开始，我们可以从任何点上开始单词。

##### 回溯算法的基本思路

1.确定Base case，即考虑一些越界情况，结果实现的情况。

2.在选择列表中进行选择

3.迭代下一个情况

4.撤销选择

这个回溯算法可以如下定义：



```python
def backtrack(x,y,index)
```


      File "C:\Users\86157\AppData\Local\Temp/ipykernel_7228/2593934997.py", line 1
        def backtrack(x,y,index)
                                ^
    SyntaxError: invalid syntax
    


x,y就是我们传进去的board的行数和列数，index为单词的索引


```python
def dfs(x,y,index):
            #base case
        
            if x<0 or x>=row or y<0 or y>=col: 、
                return False
            if board[x][y]!=word[index]:
                return False
            if index ==len(word)-1:
                return True
            
            #做选择

            board[x][y] = '#' 
            
            #进行迭代
            
            for i in [[0,1],[1,0],[0,-1],[-1,0]]:
                next_x = x+i[0]
                next_y = y+i[1]
                if dfs(next_x,next_y,index+1):
                    return True

                    
            #撤销选择
            
            board[x][y] = word[index]

            return False
        
            #开始点，可以从任一行任一列开始
            
        for i in range(row):
            for j in range(col):
                if dfs(i,j,0):
                    return True

        return False
```

如上，按照思路，第一步：确立Base case，当索引为index的单词字符刚刚和board的x行y列的字符相同时，继续走下去，如果不是，则return，程序返回上一步（即返回上一次for循环，进行下一个循环），如果继续走下去，就再次执行刚刚的内容，如此循环，直至我们可以找到一个序列和给出的单词一致，即index的长度达到了单词的长度。

让我们来看下一道一个比较简单的题目， [全排列](https://leetcode.cn/problems/permutations/)

输入：nums = [1,2,3]


输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

代码如下：


```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        track = []
        def backtrack():
            if len(track) == len(nums): #base case
                res.append(track[:])
                return
            
            for i in range(0,len(nums)):
                if nums[i] in track:
                    continue

                track.append(nums[i]) #选择
                backtrack() #迭代
                track.pop()#撤销选择
                
        backtrack()
        return res
```

还是惯常的思路，确立一个Base case,然后进行选择，进行迭代，撤销选择。

其实从回溯算法里面，我们也能发现一件事，那就是所谓“智能”的计算机，实际上是因为计算机拥有巨大的“计算能力"，它能不厌其烦的进行大量的数据处理，并从这些数据中根据我们所确立的“basecase”进行选择。

如果你跟一个不知道计算机怎么运行的人演示这个全排列算法（如果你去演示一个N皇后，解数独的例子可能更好），他们会纠结于为什么你跟电脑说：“给我一个数组[1,2,3]的全排列输出。”电脑就会如实的做到，电脑他难道是因为知道全排列的含义而去思考吗？实际上是你给他一个指令，让他去遍历这些数组组成的所有有用序列，然后返回而已。

不过，这样也可以说明，电脑是可以拥有“学习能力”的，比如你给计算机一百万个数据，让他去处理这之间的关系（而不需要你告诉它关系是什么），然后你再丢给它一个不是数据里数字，他能返回给你一个“最接近结果”的一个值，当然，这就是另一种事情了。


