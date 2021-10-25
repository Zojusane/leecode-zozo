# 240th 搜索二维矩阵II

### 题目描述

- 编写一个高效的算法来搜索 *m* x *n* 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：
  - 每行的元素从左到右升序排列
  - 每列的元素从上到下升序排列

>示例1：
>
>$$ a = \left[ \begin{matrix}  \ 1&4&7&11&15\\  \ 2&'5'&8&12&19\\  \ 3&6&9&16&22\\ \ 10&13&14&17&24\\ \ 18&21&23&26&30 \end{matrix} \right] $$
>
>输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
>输出：true
>
>题目只需要求出是否含有该元素即可.

#### 我的解法

- 由于行列都是按照升序排列,所以我们从对角线的位置遍历,当目标第一次小于对角线的值时,左上角和右下角的元素都被舍去了,因为左上角最大值为上一个对角线的值,而target大于它,右下角最小值为对角线的值,而target小于它,所以答案在右上和左下.同时需要注意的是,矩阵是长方形的,我们切割成一个个正方形,这里说的对角线也是正方形的对角线,所以需要不断地重置行向量或者列向量,并且保存下当前的行列坐标.

- 实际上这题可以先看长宽的大小,分割成正方形后,留下来的矩形直接遍历就行. 这里将留下来的矩形继续分割.直至结束.

  ```java
  class Solution {
      public boolean searchMatrix(int[][] matrix, int target) {
          int m = matrix.length;
          int n = matrix[0].length;
          int i = 0,j = 0; 
          int iplus = 0,jplus = 0;
          while(i<m||j<n){
              if(j>n-1)  {j=jplus; iplus=i;}//当列坐标超过列数时,将列坐标回溯到上一个,同时记录行坐标
              if(i>m-1)  {i=iplus; jplus=j;}//同上
              if(target>matrix[i][j]) {i=i+1;j=j+1;}//当target更大时,继续遍历
              else if(target==matrix[i][j]) return true;//恰好相等
              else{
                  for(;j<n&&matrix[iplus][j]<=target;j++)//按列元素遍历右上角,当第一位大于target的时候,后面的数将都大于target,无需遍历
                      for(int k=iplus;k<i;k++)//遍历这一列的元素 
                          if(matrix[k][j]==target) return true;
                  for(;i<m&&matrix[i][jplus]<=target;i++)//按行元素遍历左下角
                      for(int k=jplus;k<j;k++) 
                          if(matrix[i][k]==target) return true;
                  return false;//这些元素中没有的话,那就一定没有
              }
          }
          return false;//整个矩阵都小于target
      }
  }
  ```

	#### 官方解法

- 该矩阵的左下角元素和右上角元素有类似的特点,如左下角元素是这一列的最大值,也是这一行的最小值.

- 当target小于左下角元素时,target<最后一行所有元素,所以可去掉最后一行,同理,当target大于左下角元素时,target>第一列所有元素,去掉第一列,直到target等于左下角元素或者矩阵为空为止.

- 这个方法十分简单,同理用右上角来循环也是可以的

  ```java
  class Solution {
      public boolean searchMatrix(int[][] matrix, int target) {
          int m = matrix.length, n = matrix[0].length;
          int x = m-1, y = 0;//左下角元素初始化
          while (x >=0 && y <n-1) {
              if (matrix[x][y] == target) {
                  return true;
              }
              if (matrix[x][y] > target) {//左下角元素大于target,去掉最后一行
                  x--;
              } else {//小于则去掉第一列
                  y++;
              }
          }
          return false;
      }
  }
  
  作者：LeetCode-Solution
  链接：https://leetcode-cn.com/problems/search-a-2d-matrix-ii/solution/sou-suo-er-wei-ju-zhen-ii-by-leetcode-so-9hcx/
  来源：力扣（LeetCode）
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  ```

  

