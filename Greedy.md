保证每次操作都是局部最优，从而使最后得到的结果是全局最优

455. Assign Cookies(EASY)(https://leetcode.com/problems/assign-cookies/)  

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

