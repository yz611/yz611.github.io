---
layout: post
title:  "Day 1: Binary search and remove element"
date:   2023-02-17
categories: algo/leetcode
---
This is my first blog on algorithms. I started doing leetcode questions a few days ago as part of a online training camp that will last 60 days. The ability to understand the logic behind and apply them in daily work is something I have thought about developing for a long time. Hopefully, at the end of the course I will be able to have a better understanding of both the theory and practice of computer programming. I will implement my solutions in [C++](https://en.wikipedia.org/wiki/C%2B%2B).

The [first question](https://leetcode.com/problems/binary-search/) is straightforward. However, it took me some time to produce the correct condition to breaking the loop. There are two way to implement the solution.

We may use a `closed` interval:

{% highlight cpp %}
class Solution {
public:
    int search(vector `<int>`& nums, int target) {
        int lower = 0;
        int higher = nums.size()-1;

    while (lower <= higher) // "<=" as we use closed interval
        {
            int mid = (lower + higher) / 2;
            if (target == nums[mid])
            {
                return mid;
            } else if (target < nums[mid]) {
                higher = mid-1;
            // it is mid-1 as we do not want to include nums[mid] in the next loop.
            } else {
                lower = mid+1;
            }
        }
        return -1;
    }
};
{% endhighlight %}

Or we can use a `left-closed, right-open` interval:

{% highlight cpp %}
class Solution {
public:
    int search(vector `<int>`& nums, int target) {
        int lower = 0;
        int higher = nums.size();
        while (lower < higher)
        {
            int mid = (lower + higher) / 2;
        if (target == nums[mid])
            {
                return mid;
            } else if (target < nums[mid]) {
                higher = mid;
            } else {
                lower = mid+1;
            }
        }
        return -1;
    }
};
{% endhighlight %}

The time complexity is `O(logn)` and space complexity `O(1)`.

The [second question](https://leetcode.com/problems/remove-element/) was not difficult to solve by brute force. However, to achieve a time complexity of `O(n)`, one should exploit the so called `two pointer` method:

{% highlight cpp %}
class Solution {
public:
    int removeElement(vector `<int>`& nums, int val) {
        int slowPtr = 0;
        for (int fastPtr=0; fastPtr < nums.size(); ++fastPtr)
        {
    if (val != nums[fastPtr]) {
                nums[slowPtr++] = nums[fastPtr];
            }
        }
        return slowPtr;
    }
};
{% endhighlight %}

I was stunned by the simplicity of this solution. It made use of post-increment in C++, and presented insights into the `fast-slow pointers` version of two pointers method. In this case, the fast pointer checks for the condition (i.e. if the value equals target), and the slow pointer inserts the value in place where it has o

There is another version which initializes the two pointers at the front and rear ends of the array:

{% highlight cpp %}
class Solution {
public:
    int removeElement(vector `<int>`& nums, int val) {
        int leftIndex = 0;
        int rightIndex = nums.size() - 1;
        while (leftIndex <= rightIndex) {
            while (leftIndex <= rightIndex && nums[leftIndex] != val){
                ++leftIndex;
            }
            while (leftIndex <= rightIndex && nums[rightIndex] == val) {
                -- rightIndex;
            }
            if (leftIndex < rightIndex) {
                nums[leftIndex++] = nums[rightIndex--];
            }
        }
        return leftIndex;
    }
};
{% endhighlight %}
