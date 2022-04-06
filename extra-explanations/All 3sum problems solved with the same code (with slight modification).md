All 3sum problems on Leetcode:
* [3sum](https://leetcode.com/problems/3sum/)
* [3sum with multiplicity](https://leetcode.com/problems/3sum-with-multiplicity)
* [3sum closest](https://leetcode.com/problems/3sum-closest/)
* [3sum smaller](https://leetcode.com/problems/3sum-smaller/) [💲subscription needed!] 

This article has solutions to the above first 3 problems, all of which can be solved using **[Two Pointers](https://leetcode.com/tag/two-pointers/)** approach, with just few modifications.

<br>

**C++ solutions:**

**1. 3sum**

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        
        if (n <= 2) return {};
        
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        
        for (int i = 0; i < n-2; i++) {
            
            /* MODIFICATION 1:
            Here we add some conditions just to
            reduce the time required to search 
            otherwise you'll get solution, but it's TLE! */
            if (i == 0 || (i > 0 && nums[i] != nums[i-1])) {
            
                int T = 0 - nums[i];
                int j = i+1, k = n-1;

                while (j < k) {
                    if (nums[j] + nums[k] < T)
                        j++;
                    else if (nums[j] + nums[k] == T) {
                        ans.emplace_back(vector<int>{nums[i], nums[j], nums[k]});

                        /* MODIFICATION 2:
                        Take care of duplicates: */
                        while (j < k && nums[j] == nums[j+1]) j++;
                        while (j < k && nums[k] == nums[k-1]) k--;

                        j++, k--;

                    } else if (nums[j] + nums[k] > T)
                        k--;
                }
            
            }
        }
        
        return ans;
    }
};
```

<br>

**2. 3sum closest**

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int n = nums.size();
        
        if (n <= 2) return 0;
        
        sort(nums.begin(), nums.end());
        int closest_sum = 0;
        int min_diff = INT_MAX;
        
        for (int i = 0; i < n; i++) {

            int T = target - nums[i];
            int j = i+1, k = n-1;
            
            while (j < k) {
                
                /* MODIFICATION: 
                Keep track of the sum every time,
                because we need to find the closest sum
                */
                int curr_sum = nums[i] + nums[j] + nums[k];
                int curr_diff = abs(curr_sum - target);
                if (curr_diff < min_diff) {
                    closest_sum = curr_sum;
                    min_diff = curr_diff;
                }
                
                if (nums[j] + nums[k] < T)
                    j++;
                else if (nums[j] + nums[k] > T)
                    k--;
                else if (nums[j] + nums[k] == T) {
                    closest_sum = nums[i] + nums[j] + nums[k];
                    return closest_sum;
                }
            }
        }
        
        return closest_sum;
    }
};
```

<br>

**3. 3sum with multiplicity**

Solution with explanation can be found [here](https://leetcode.com/problems/3sum-with-multiplicity/discuss/1918974/Comprehensive-explanation-in-comments-C++-Two-pointer-approach)

```
class Solution {
public:
    int threeSumMulti(vector<int>& arr, int target) {
        int n = arr.size();
        if (n <= 2) return -1;
        
        int MOD = 1'000'000'007;
        long ans = 0;
        sort(arr.begin(), arr.end());
        
        for (int i = 0; i < n; i++) {
            int T = target - arr[i];
            int j = i+1, k = n-1;

            while (j < k) {
                
                /* MODIFICATION: 
                Here we need to handle additional cases
                where there are duplicates.
                Also, a small modification is that we need
                to store the answer as answer%10^9 + 7
                */
                if (arr[j] + arr[k] < T)
                    j++;
                else if (arr[j] + arr[k] > T)
                    k--;
                else if (arr[j] + arr[k] == T && arr[j] != arr[k]) {
                    
                    int left = 1, right = 1;
                    while (j+1 < k && arr[j] == arr[j+1]) {
                        left++;
                        j++;
                    }
                    while (k-1 > j && arr[k] == arr[k-1]) {
                        right++;
                        k--;
                    }
                    ans += left*right;
                    ans %= MOD;
                    j++, k--;
                } else if (arr[j] + arr[k] == T && arr[j] == arr[k]) {
                    
                    int M = (k-j+1) * (k-j) / 2;
                    ans += M;
                    ans %= MOD;
                    break;
                }
            }
        }
        
        return (int)ans;
    }
    
};
```

<br>

For all the above solutions:
Time complexity: **O(n<sup>2</sup>)**
Space complexity: **O(1)**

<br>
