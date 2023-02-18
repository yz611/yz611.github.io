---
layout: post
title:  "Day 2: Squares of a sorted array, minimum size subarray sum, and spiral matrix"
date:   2023-02-17
categories: algo/leetcode
---

Three questions today. Much harder and cannot be solved independently. I am aware of my current level and hope for progress. 

The [First question](https://leetcode.com/problems/squares-of-a-sorted-array/) can be solved trivially by squaring and sorting again. This will make the time complexity `O(nlogn)`, which seems fine. Still, we can come up with a better solution with `two pointers` and extra memory.

The solution is as follows:
{% highlight cpp %}
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> rtn(nums.size(), 0);
        int k = nums.size()-1;
        for (int i=0, j=nums.size()-1; i<=j;)
        {
            if (nums[i]*nums[i] > nums[j]*nums[j])
            {
                rtn[k--] = nums[i]*nums[i];
                i++;
            } else {
                rtn[k--] = nums[j]*nums[j];
                j--;
            }
        }
        return rtn;
    }
};
{% endhighlight %}
This has a both time and space complexity of `O(n)`.

The [second question](https://leetcode.com/problems/minimum-size-subarray-sum/) was not a easy one. It is 
a use case of `sliding-window` method.

Sliding-window method is just a variation of `fast-slow pointer` method, which operate on both elements the pair point to every time we increment the fast pointer. In this case, the operation is to `read` the value dereferenced by the fast pointer and `add` it to `sum`, a variable initialized to 0 in the outer scope. Then, we `read` and `subtract` the value dereferenced by the slow pointer again, and increment the pointer until `sum` is `less than` the `target` we set. After that, we should be confident to enter the next cycle. In each operation, we take the `minimum size` by `comparing` the length of the subarray between the pair of pointers with `result`, which was initialized to infinity in the outer scope.

The following implementation shows the process.
{% highlight cpp %}
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int sum = 0;
        int result = INT32_MAX;
        int i = 0;
        int subLength = 0;
        for (int j=0; j < nums.size(); ++j)
        {   
            sum += nums[j];
            while (sum >= target)
            {
                subLength = (j-i+1);
                result = result < subLength ? result : subLength;
                sum -= nums[i++];
            }
        }
        return result < INT32_MAX ? result : 0;
    }
};
{% endhighlight %}
I highlighted the operations we have carried out on the elements. Basically, we were trying to "catch up" with the fast pointer while making sure `sum` and `result` were correctly populated. 

The [third question](https://leetcode.com/problems/spiral-matrix-ii/) is convoluted. Nevertheless, the logic
itself is not totally unconceivable.

As we spiral into the center of the matrix, we are 
actually populating the layers of blocks with the same logic applied to each of them. The only differences are starting x and y indices, offset and values. The value is just a count of blocks that increases from zero, and the indices and offset should be incremented / decremented as we finish one cycle of the loop. 

The solution is as follows:
{% highlight cpp %}
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n, 0));
        int startx = 0;
        int starty = 0;
        int offset = 1;
        int count = 1;
        int loop = n/2;
        while(loop--)
        {
            int i=startx;
            int j=starty;
            
            for (j=starty;j < n-offset; ++j)
                result[startx][j] = count++;
            
            for (i=startx;i < n-offset; ++i)
                result[i][j] = count++;
            
            for (; j > starty; --j)
                result[i][j] = count++;

            for (; i > startx; --i)
                result[i][j] = count++;

            startx++;
            starty++;        
            offset++;
        }
        if (n%2)
            result[startx][starty] = count;
        return result;
        
    }
};
{% endhighlight %}



