保证每次操作都是局部最优，从而使最后得到的结果是全局最优  

## 1分配问题

### 455. Assign Cookies(EASY)(https://leetcode.com/problems/assign-cookies/)  

Input: g = [1,2,3], s = [1,1]  
Output: 1  
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3.   
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.  
You need to output 1.  

Input: g = [1,2], s = [1,2,3]  
Output: 2  
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2.   
You have 3 cookies and their sizes are big enough to gratify all of the children,   
You need to output 2.  

策略：  
    给剩余孩子里最小饥饿度的孩子分配最小的能满足要求的饼干
````
    class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0;
        int j = 0;
        // 不需要单独写一个res来记录满足了多少个孩子，i就能直接表示满足的孩子数量
        while(i < g.length && j < s.length)
        {
            if(s[j] >= g[i])
            {
                i++;
            }
            j++;
        }
        return i;
    }
  }
````
### 135.Candy(Hard) (https://leetcode.com/problems/candy/)

You are giving candies to these children subjected to the following requirements:  

Each child must have at least one candy.  
Children with a higher rating get more candies than their neighbors.  
  
Return the minimum number of candies you need to have to distribute the candies to the children.
````
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
````
题解：核心思路---高rating多1个candy的最优策略。此贪心问题主要是分别考虑左右两边的最优解，再总合左右两边就能得到全局最优解。

从左往右满足。此时可知对于当前的第i个孩子，他所得糖果数为只考虑左边的最小数（从1开始递增1）.再从右往左满足高rating多1个candy，可得当前的最小的糖果数，只考虑右边。整体问题需要考虑左右2边，故取最大值。
````
class Solution {
    public int candy(int[] ratings) {
        int[] arr = new int[ratings.length];
        Arrays.fill(arr,1);
        for(int i = 1; i < ratings.length; i++){
            if(ratings[i] > ratings[i-1]) arr[i] = arr[i-1] + 1;
        }
        for(int i = ratings.length - 2; i >= 0; i--){
            if(ratings[i] > ratings[i+1]){
                arr[i] = Math.max(arr[i],arr[i+1]+1);
            }
        }
        int sum = 0;
        for(int c : arr) sum += c;
        return sum;
    }
}

// 1 2 3 4 5 3 4 5 2 1 3 5 
// 1 2 3 4 5 1 2 3 2 1 2 3
````
## 2区间问题  
### 435.Non-overlapping Intervals(Medium)(https://leetcode.com/problems/non-overlapping-intervals/)
Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.  

````
Input: [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of intervals are non-overlapping.

Input: [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of intervals non-overlapping.

Input: [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
````
题解：贪心策略--> 按start和end排序，当发现前后有overlapping时，选择消除end比较大的element，因为这样可以最大程度上给后面的start留下不重叠的空间。
贪心策略应该是每次选取结束时间最早的活动。直观上也很好理解，按这种方法选择相容活动为没有安排的活动留下尽可能多的时间。这也是把各项活动按照结束时间单调递增排序的原因。
````
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals,(o1,o2)->o1[0] == o2[0]? o1[1] - o2[1] : o1[0] - o2[0]);
        int res = 0;
        for(int i = 1 ; i < intervals.length; i++)
        {
            if(intervals[i][0] < intervals[i-1][1])
            {         
                res++;
                intervals[i][1] = Math.min(intervals[i][1],intervals[i-1][1]);
            }
        }
        return res;
    }
}
/*
  1 2 3 4
   -
     -
       -
   ---    
    */
````

## 3练习
### 605. Can Place Flowers(https://leetcode.com/problems/can-place-flowers/)  
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in adjacent plots.  

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.  
````
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true

Input: flowerbed = [1,0,0,0,1], n = 2
Output: false
````
题解：贪心策略--> 尽早改变，越后只会压缩可改变的区域。

````
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        // 考虑 n = 0 / [0]时，必定返回true。 
        if(n == 0 || (flowerbed.length == 1 && flowerbed[0] == 0)) return true;
        int index = 0;
        while(index < flowerbed.length){
            int pre = index == 0 ? 0 : flowerbed[index-1];
            int next = index == flowerbed.length - 1 ? 0 : flowerbed[index + 1];
            if(pre == 0 && next == 0 && flowerbed[index] == 0)
            {
                flowerbed[index++] = 1;
                n--;
            }
            if(n == 0) return true;
            index++;
        }
        return false;
    }
}
````

总结：当遇到前后对比，或者edge case不方便处理的情况，可以使用一个pre和next来方便处理edge case，简化代码。

### 452. Minimum Number of Arrows to Burst Balloons(Medium) (https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.  

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.  

Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.  

````
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).

Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4

Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
````
题解 ： 贪心策略 --> 维护一个局部箭头区域，只要维护end即可，因为数组是按照end排序的，只要下一个start小于维护区域的end，那可以被同一根箭一起射爆。如果大于该区域的右边界，则表明和之前维护的区域不能被同一根箭射爆。怎arrow num++。
````
class Solution {
    public int findMinArrowShots(int[][] points) {
        int n = points.length;
        if(n == 0) return 0;
        // 考虑到-2^31到 2^31 -1。所以排序时要考虑溢出的状况。
        Arrays.sort(points,(o1,o2)->o1[1] > o2[1] ? 1 : -1);
        int res = 0;
        for(int i = 1; i < n; i++){
            if(points[i][0] <= points[i-1][1])
            {
                points[i][1] = Math.min(points[i-1][1],points[i][1]);
            }else{
                res++;
            }
        }
        res++;
        return res;
    }
}
/*
         2-6
        1--------8
             7 - - 12
              10 ---- 16
*/         
````
总结：注意溢出的情况。排序过程中的溢出容易被忽视。  
      注意是否需要对x坐标进行排序。
      
      
### 763.Partition Labels(Medium)(https://leetcode.com/problems/partition-labels/)
A string S of lowercase English letters is given. We want to partition this string into as many parts as possible so that each letter appears in at most one part, and return a list of integers representing the size of these parts.

````
Input: S = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits S into less parts.
````
题解：贪心策略--> 在R里即这一部分的最优解。
````
class Solution {
    public List<Integer> partitionLabels(String S) {
        int[] map = new int[26];
        for(int i = 0; i < S.length(); i++){
            map[S.charAt(i)-'a'] = i;
        }
        int r = 0, last = -1;
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i < S.length(); i++){
            r = Math.max(r,map[S.charAt(i)-'a']);
            if(i == r)
            {
                res.add(i-last);
                last = i;
            }
        }
        return res;
    }
}
````
