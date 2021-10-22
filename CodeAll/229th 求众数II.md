# 229th 求众数II

### 题目描述

- 给定一个大小为 *n* 的整数数组，找出其中所有出现超过 `⌊ n/3 ⌋` 次的元素。

  > 示例
  >
  > ```
  > 输入：[1,1,1,3,3,2,2,2]
  > 输出：[1,2]
  > ```

#### 我的解法

- 这是一道中等题, 但是我并没有想到很巧妙的解法,如果是一般解法的话,这题应该是简单题.

- 我的解答16ms,击败了java的5%,所以应该有更好的解法,我们之后看官解.

  ```java
  class Solution {
      public List<Integer> majorityElement(int[] nums) {
          Map <Integer,Integer> major = new HashMap<>();//累计各数字出现次数
          List<Integer> in = new ArrayList<>();
          int n = nums.length;
          for(int i=0;i<n;i++){
              int value= major.getOrDefault(nums[i],0);//初始化0
              major.put(nums[i],value+1);//累加
              if(major.get(nums[i])>n/3) {
                  in.add(nums[i]);//达到目标后添加
                  major.replace(nums[i],-n);//防止重复添加,设为-n后不可能累加回来,这里可以优化
              }
              if(in.size()==2) return in;//可以证明,最多只有两个数是"众数"
          }
          return in;
      }
  }
  ```

  #### 评论解法

  - 看评论说是不使用Map的解法,有个摩尔投票法.解决的问题是找到数组中大于数组长度一半的数.

  - 思想是投票, 而且被投票的人只有一个, 当下一个数和它一样, 票数加一, 否则票数减一. 票数为0时, 将换下一位为被投票人. 最终有票的为结果. 

  - 从头开始遍历数组,每个数与目前最多的那个数作比较,如果相等,则数对应的票+1,否则减一.若最终票大于0,查看最后的数是否占总体的一半以上.为什么这是正确的呢,因为当票数重置0的时候, 也就是说前面的数中不存在占前面的数一半的数,这个时候占了总体一半以上的数必定会占后续的数的一半以上. 可以用反证法,假设这个数不占前面数的一半,也不占后面数的一半,那么他就不会占总体的一半.

  - 从总体来看,我们可以这样想,超过总体一半的数,就算一个一个的对子,最终留下来的还是这个数,就是说如果存在,那就一定是那个数.

    ```java
    public int majorityElement(int[] nums) {  
      
        int majority = -1;  
      
        int count = 0;  
      
        for (int num : nums) {  
            if (count == 0) { //换新的数,前半程不存在占一半以上的数 
                majority = num;  
                count++;  
            } else {  
                if (majority == num) {  
                    count++;  
                } else {  
                    count--;  
                }  
            }  
        }  
      
        int counter = 0;  
        if (count <= 0) {  
            return -1;  
        } else {  
            for (int num : nums) {  
                if (num == majority) counter ++;  
            }  
        }  
      
        if (counter > nums.length / 2) {  //判断是否正确,唯一的可能性
            return majority;  
        }  
      
        return -1;  
    } 
    ```

    我们通过摩尔投票法再看这道题,就是将被投票者变为两位,接下来的数拥有肯定和否定的选择,尝试着写类似的代码.

    ```java
    class Solution {
        public List<Integer> majorityElement(int[] nums) {
            List<Integer> in = new ArrayList<>();
            int n = nums.length;
            int m1=Integer.MAX_VALUE,m2=Integer.MAX_VALUE;//设置两个被投票者
            int count1=0,count2=0;
            for(int i=0;i<n;i++){
                if(count1==0&&nums[i]!=m2) {m1=nums[i];count1++;continue;}//重新设置被投票的人,注意需要判断是否是第二阵营的人
                if(count2==0&&nums[i]!=m1) {m2=nums[i];count2++;continue;}//同上
                if(nums[i]==m1) count1++;
                else if(nums[i]==m2) count2++;
                else {count1--;count2--;}//都不是则各减一票
            }
            if (count1<=0&&count2<=0) return in;
            count1=count2=0;
            for(int i=0;i<n;i++){
                if(m1==nums[i]) count1++;
                if(m2==nums[i]) count2++;
            }
            if(count1>n/3) in.add(m1);//判断这两个阵营是否是众数
            if(count2>n/3) in.add(m2);
            return in;
        }
    }//最终速度2ms只打败了50%
    ```

    ####  官方解法

    > 官方使用的就是上述两种方法,可能略有差异
    >
    > ```java
    > class Solution {
    >     public List<Integer> majorityElement(int[] nums) {
    >         HashMap<Integer, Integer> cnt = new HashMap<Integer, Integer>();
    > 		for (int i = 0; i < nums.length; i++) {
    >         if (cnt.containsKey(nums[i])) {
    >             cnt.put(nums[i], cnt.get(nums[i]) + 1);
    >         } else {
    >             cnt.put(nums[i], 1);
    >         }
    >     }
    >     List<Integer> ans = new ArrayList<>();
    >     for (int x : cnt.keySet()) {
    >         if (cnt.get(x) > nums.length / 3) {
    >             ans.add(x);
    >         }
    >     }
    >    
    >     return ans;
    > }
    > //解法二
    > class Solution {
    >     public List<Integer> majorityElement(int[] nums) {
    >         int element1 = 0;
    >         int element2 = 0;
    >         int vote1 = 0;
    >         int vote2 = 0;
    > 
    >         for (int num : nums) {
    >             if (vote1 > 0 && num == element1) { //如果该元素为第一个元素，则计数加1
    >                 vote1++;
    >             } else if (vote2 > 0 && num == element2) { //如果该元素为第二个元素，则计数加1
    >                 vote2++;
    >             } else if (vote1 == 0) { // 选择第一个元素
    >                 element1 = num;
    >                 vote1++;
    >             } else if (vote2 == 0) { // 选择第二个元素
    >                 element2 = num;
    >                 vote2++;
    >             } else { //如果三个元素均不相同，则相互抵消1次
    >                 vote1--;
    >                 vote2--;
    >             }
    >         }
    > 
    >         int cnt1 = 0;
    >         int cnt2 = 0;
    >         for (int num : nums) {
    >             if (vote1 > 0 && num == element1) {
    >                 cnt1++;
    >             }
    >             if (vote2 > 0 && num == element2) {
    >                 cnt2++;
    >             }
    >         }
    >         // 检测元素出现的次数是否满足要求
    >         List<Integer> ans = new ArrayList<>();
    >         if (vote1 > 0 && cnt1 > nums.length / 3) {
    >             ans.add(element1);
    >         }
    >         if (vote2 > 0 && cnt2 > nums.length / 3) {
    >             ans.add(element2);
    >         }
    > 
    >         return ans;
    >     }
    > }
    > 
    > ```
    > 作者：LeetCode-Solution
    > 链接：https://leetcode-cn.com/problems/majority-element-ii/solution/qiu-zhong-shu-ii-by-leetcode-solution-y1rn/
    > 来源：力扣（LeetCode）
    > 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

  