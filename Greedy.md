保证每次操作都是局部最优，从而使最后得到的结果是全局最优

455. Assign Cookies(EASY)(https://leetcode.com/problems/assign-cookies/)

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

